# AlPgnTxNew_TP

* * * * * * * * * *

## Introduction

The function block **AlPgnTxNew_TP** is used to transmit data over the ISOBUS/CAN bus based on Parameter Group Numbers (PGN). It enables the registration of a specific PGN and the subsequent transmission of data packets whenever a local request event (REQ) occurs. The block is part of the `isobus::pgn::tx` package and is intended for messages with **more than 8 data bytes** sent over the ISO transport protocol (TP), up to 1785 bytes. For up to 8 bytes there is [`AlPgnTxNew8B`](AlPgnTxNew8B.md).
![AlPgnTxNew_TP](AlPgnTxNew_TP.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (Type: `EInit`): Initializes the service. Sets the block to its initial state.
- **install** (Type: `Event`): Installs or registers a transmit PGN. Parameters such as PGN number, destination, data size, and priority are adopted.
- **REQ** (Type: `Event`): Requests data transmission (unacknowledged transmission). Uses the buffer provided in input `Data`.

### **Event Outputs**

- **INITO** (Type: `EInit`): Acknowledges initialization.
- **installO** (Type: `Event`): Acknowledges that the installation process has completed. Returns `PGN_handle`.
- **CNF** (Type: `Event`): Confirmation that the data was sent successfully.
- **dataERR** (Type: `Event`): Signals an error during data transmission or access to the data.
- **pgnERR** (Type: `Event`): Signals an error regarding the PGN configuration or registration.

### **Data Inputs**

- **u32Pgn** (Type: `UDINT`): The parameter group number (PGN) to be sent. The range is between 0 and 0x3FFFF.
- **NmDestin** (Type: `isobus::pgn::ISONETEVENT_T`): Defines the communication partner or the destination in the network (Network Management Destination).
- **u16DaSize** (Type: `UINT`): The **length actually sent** for the PGN in bytes, at most 1785. It is fixed at `install` and is not taken from the size of the array `Data`. If `Data` is shorter, the missing bytes are filled with `16#FF`, a longer array is truncated. With `u16DaSize` = 0 a PGN without data bytes is sent, no TP takes place.
- **u8Priority** (Type: `USINT`): The default priority of this PGN (0 to 7), where 0 is the highest priority. The default value is 7.

### **Data Outputs**

- **PGN_handle** (Type: `INT`): Return value after installation. A valid handle on success, or `HANDLE_UNVALID` in case of failure.
- **dataERRC** (Type: `INT`): Error code output when the event `dataERR` is triggered. * **pgnERRC** (Type: `INT`): Error code output when the event `pgnERR` is triggered.

### **InOut Variables**

- **Data** (Type: `BYTE`, ArraySize: `*`): The data buffer (variable-size byte array, `ARRAY[*] OF BYTE`). The payload data to be sent is stored here. The length sent is determined by `u16DaSize`, not by the size of the array. Declaring it as `InOut` enables efficient memory management without unnecessary copy operations.

### **Adapters**

- This function block does not use any explicit adapter interfaces.

## Functionality

The module operates in two main phases:

1. **Configuration (Installation):**

- First, the module must be initialized via `INIT`.
- Then, the PGN is configured via the event `install`. This process defines the PGN ID (`u32Pgn`), the target (`NmDestin`), the size (`u16DaSize`), and the priority (`u8Priority`).
- After successful installation, the event `installO` is triggered, and a `PGN_handle` handle is provided. This handle represents the registered PGN in the system.

1. **Data Transfer:**

- Data must be provided in the `Data` array before it can be sent.
- The `REQ` event triggers the sending process.
- `CNF` is triggered upon successful transmission.
- If problems occur (e.g., invalid data length or bus error), `dataERR` is triggered with the corresponding error code `dataERRC`.

## Technical Specifications

- **Variable Array Size:** The `Data` input is defined as the `InOut` variable with `ArraySize="*"`. The function block works with data buffers of varying sizes. The length sent is determined solely by `u16DaSize`, though.
- **Limit of 1785 bytes:** At most 1785 bytes can be sent through this block (255 packets of 7 bytes each). The extended transport protocol (ETP) for larger amounts is not available to the block.
- **Does not answer requests:** The block only sends on the event `REQ`. It does not answer an incoming Request for PGN. See [Request for PGN, response and transport protocol](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/isobus-other-docs/en/latest/Request-Antwort-und-Transportprotokoll/).
- **Error Separation:** The function block explicitly distinguishes between errors during PGN setup (`pgnERR`) and errors during runtime data transmission (`dataERR`). This significantly simplifies debugging.
- **ISOBUS Types:** The function block imports specific ISOBUS data types (`isobus::pgn::ISONETEVENT_T`), which requires tight integration into corresponding ISOBUS stacks.

## When `AlPgnTxNew_TP`, when `AlPgnTxNew8B`?

| | `AlPgnTxNew8B` | `AlPgnTxNew_TP` |
| --- | --- | --- |
| Data length | 0 to 8 bytes | over 8 bytes, up to 1785 bytes |
| Transmission | one CAN frame | transport protocol in several frames |
| Destination `NmDestin` global | broadcast | **BAM** (no feedback) |
| Destination `NmDestin` one device | specific | **RTS/CTS** (the receiver controls and acknowledges) |
| `Data` | fixed 8 bytes | `ARRAY[*] OF BYTE`, length from `u16DaSize` |

The block chooses BAM or RTS/CTS itself according to the destination. If you send no more than 8 bytes, use `AlPgnTxNew8B`.

!!! warning "No receiver for more than 8 bytes yet"
    The firmware accepts TP messages, but the existing receive blocks (`AlPgnRxNew8B` and relatives) only pass 8 bytes on to the application. A receive block for longer PGNs (`AlPgnRxNew_TP`, up to 1785 bytes) is in progress and not yet tested. Two boards using the blocks of this library can therefore not yet exchange a message over 8 bytes.

## State Overview

The function block has internally implicit states defined by the availability of a valid `PGN_handle`:

- **Not Initialized:** Before `INIT`.
- **Initialized, Not Installed:** After `INIT`, but before successful `install`. Send requests (`REQ`) would fail here.
- **Installed / Ready for Operation:** A valid handle has been created. Data can be sent via `REQ`.

## Application Scenarios

- **ISOBUS Device Development:** Implementation of virtual terminals or task controllers that need to send specific PGNs.
- **Sensor Data Transmission:** A sensor module sends measured values to the CAN bus cyclically or based on events.
- **Control Commands:** Sending control messages to actuators in the network (e.g., valves or motors) with a defined priority.

## ⚖️ Comparison with Similar Function Blocks

- **[`AlPgnTxNew8B`](AlPgnTxNew8B.md):** Sends up to 8 bytes in one CAN frame. `AlPgnTxNew_TP` is the variant for more than 8 bytes over the transport protocol (see the table above).
- **Standard CAN_WRITE:** Compared to a generic `CAN_WRITE` function block, `AlPgnTxNew_TP` abstracts the complexity of PGN management (priority, data length, handle management) and is more specifically tailored to the J1939/ISOBUS protocol.
-

## 🛠️ Related Exercises

- [Exercise_128b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_128b/)

## Conclusion

The **AlPgnTxNew_TP** is a specialized and robust function block for sending messages in ISOBUS networks. By separating configuration (`install`) and transmission (`REQ`) and utilizing reference data buffers (`InOut`), it is ideally suited for resource-efficient control applications that require precise control over PGN parameters.

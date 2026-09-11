# A2X2_CLIENT_2_0_SUBSCRIBE_2_PLUG

![A2X2_CLIENT_2_0_SUBSCRIBE_2_PLUG](./A2X2_CLIENT_2_0_SUBSCRIBE_2_PLUG.svg)

* * * * * * * * * *

## Introduction

The `A2X2_CLIENT_2_0_SUBSCRIBE_2_PLUG` is a composite function block that bridges a bidirectional A2X2 adapter plug with an OPC-UA server. It writes two Boolean values (UP and DOWN) to a remote node via the `CLIENT_2_0` write service and continuously reads two Boolean values from another remote node via the `SUBSCRIBE_2` subscription service. Each Boolean direction is individually buffered using its own `E_D_FF` flip‑flop, ensuring reliable edge‑triggered data transfer between the adapter interface and the network services.

This block is the plug‑side mirror of `A2X2_CLIENT_2_0_SUBSCRIBE_2`, which uses a socket. It is designed for applications where an OPC-UA server must be remotely controlled and monitored through a bidirectional adapter interface, such as in industrial automation and IoT gateways.

## Interface Structure

### **Event Inputs**

| Event   | Type    | Comment                          | With |
|---------|---------|----------------------------------|------|
| `INIT`  | `EInit` | Initialization                   | `QI` |

### **Event Outputs**

| Event   | Type    | Comment                               | With                              |
|---------|---------|---------------------------------------|-----------------------------------|
| `INITO` | `EInit` | Initialization Confirm                | –                                 |
| `CNF`   | `Event` | `QO` / `STATUS` updated               | `QO`, `STATUS_WRITE`, `STATUS_READ` |

### **Data Inputs**

| Name        | Type      | Comment                                                                 |
|-------------|-----------|-------------------------------------------------------------------------|
| `QI`        | `BOOL`    | Enable the block. `TRUE` starts the client and subscribe services.     |
| `ID_WRITE`  | `WSTRING` | Remote target address (ACTION=WRITE) for both Boolean values.          |
| `ID_READ`   | `WSTRING` | Locally monitored state node (ACTION=READ) for both Boolean values.    |

### **Data Outputs**

| Name           | Type      | Comment                                                             |
|----------------|-----------|---------------------------------------------------------------------|
| `QO`           | `BOOL`    | `TRUE` only if both `WRITE_CLIENT.QO` and `READ_SUBSCRIBE.QO` are `TRUE`. |
| `STATUS_WRITE` | `WSTRING` | Status of the `WRITE_CLIENT` instance.                              |
| `STATUS_READ`  | `WSTRING` | Status of the `READ_SUBSCRIBE` instance.                            |

### **Adapters**

| Name | Type                                        | Comment                                             |
|------|---------------------------------------------|-----------------------------------------------------|
| `IO` | `adapter::types::bidirectional::A2X2`       | UP/DOWN BOOLs to send (out), UP/DOWN BOOLs received (in). |

## Functionality

The block operates as a bidirectional data bridge between the A2X2 adapter plug and an OPC-UA server. Two independent paths handle the write and read directions:

**Write Path (PLC → OPC-UA):**  
The adapter plug provides two input Booleans, `DI_UP` and `DI_DOWN`. When the plug raises the corresponding event (`EI_UP` or `EI_DOWN`), the associated `E_D_FF` latch captures the current value on its `D` input and triggers its output event (`EO`). This event activates `WRITE_CLIENT.REQ`, causing the client to send the latched value(s) (`SD_1` for UP, `SD_2` for DOWN) to the remote OPC-UA node specified by `ID_WRITE`. The client’s `CNF` event indicates completion or error.

**Read Path (OPC-UA → PLC):**  
The `SUBSCRIBE_2` FB continuously listens to the remote node given by `ID_READ`. When new data arrives (event `IND`), the two received Booleans (`RD_1` and `RD_2`) are latched into the corresponding `E_D_FF` flip‑flops. The outputs of these flip‑flops (`Q`) are placed on `DO_UP` and `DO_DOWN`, and the respective output events (`EO_UP`, `EO_DOWN`) are triggered on the adapter plug, notifying the consumer that new values are available.

**Initialization and Monitoring:**  

- The `INIT` event starts the `READ_SUBSCRIBE` first; after its `INITO`, the `WRITE_CLIENT` is initialized. The overall `INITO` is emitted when both are ready.
- The `AND_QO` logic combines the `QO` outputs of both network blocks. `QO` is `TRUE` only when both the client and the subscriber report operational health. The `CNF` event is triggered by the `AND_QO` block whenever any of the two blocks updates its status. This allows external logic to monitor the combined service status.

## Technical Features

- **Composite type** – Built from standard 4diac FBs (`CLIENT_2_0`, `SUBSCRIBE_2`, `E_D_FF`, `AND_BOOL_2`).
- **Edge‑triggered buffering** – Each Boolean direction uses a dedicated `E_D_FF` to reliably capture data changes on the adapter side and on the subscription side.
- **Independent addressing** – The write and read targets are configured separately via `ID_WRITE` and `ID_READ`, allowing different OPC-UA nodes for command and feedback.
- **Combined status** – The `QO` output reflects the overall health of both communication channels.
- **Event‑driven** – No continuous polling; all transfers are event‑based, minimizing network traffic.
- **Initialization sequence** – Ensures subscribe becomes active before client writes are attempted.

## State Overview

There is no explicit state machine; the block’s behaviour is event‑driven. However, the following operational states can be inferred:

- **Idle** – `QI = FALSE` or initialization not completed. No network activity.
- **Initializing** – After `INIT`, the subscribe service is being started, followed by the client. `QO` is `FALSE`.
- **Active** – Both `WRITE_CLIENT` and `READ_SUBSCRIBE` have `QO = TRUE`. The block is writing and reading data as events arrive.
- **Degraded / Error** – Either the client or subscriber reports a failed `QO` (`FALSE`). The combined `QO` becomes `FALSE`, and the corresponding `STATUS` output contains diagnostic information.

## Application Scenarios

- **Remote I/O control** – Controlling two output bits on a remote PLC or device via OPC-UA while simultaneously monitoring two input bits.
- **HMI / SCADA integration** – Bridging an A2X2 adapter to an OPC-UA server for supervisory control and data acquisition.
- **Gateway between protocols** – Using the adapter to interface with proprietary fieldbus systems while exposing the same signals through OPC-UA.
- **Redundant monitoring** – The combined status (`QO`) can be used to trigger alarms if either the write or read channel fails.

## Comparison with Similar Blocks

- **`A2X2_CLIENT_2_0_SUBSCRIBE_2` (Socket version)** – Operates identically but uses an A2X2 socket instead of a plug. The choice depends on the direction of data flow in the surrounding network; this block is intended for use in a system that provides the adapter (plug side).
- **Simple write/read blocks** – Standalone `CLIENT_2_0` and `SUBSCRIBE_2` blocks require manual management of events and data. This composite block encapsulates the necessary buffering and event linking, simplifying integration.
- **Alternative adapter types** – Blocks for other adapter types (e.g., single Boolean or analog) would have different interfaces but follow a similar pattern.

## Conclusion

The `A2X2_CLIENT_2_0_SUBSCRIBE_2_PLUG` provides a clean, reusable solution for bridging two Boolean values from an adapter plug to an OPC-UA server and back. Its event‑driven design, combined with flip‑flop buffering and a unified status output, makes it well suited for real‑time industrial applications where reliable and responsive data exchange is required. The block is fully self‑contained and can be integrated directly into any 4diac application.

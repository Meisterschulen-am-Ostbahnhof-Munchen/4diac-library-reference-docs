# Q_ActiveMask_AUI

![Q_ActiveMask_AUI](./Q_ActiveMask_AUI.svg)

* * * * * * * * * *
## Introduction

The **Q_ActiveMask_AUI** function block is an AUI (Adapter User Interface) wrapper for the core block **Q_ActiveMask**. It provides a convenient, unidirectional adapter–based interface to change the active mask on an ISO 11783‑6 (ISOBUS) Virtual Terminal. The block encapsulates the functionality of the underlying service, translating adapter signals into the internal request/confirm protocol of the wrapped block.

This wrapper is designed for applications that already use the unidirectional AUI adapter type for data exchange, simplifying integration into existing adapter‑based networks. It handles the complete command sequence to change the active mask, returning both the status of the operation and a result code.

## Interface Structure

### **Event Inputs**
| Event   | Type  | Comment                    |
|---------|-------|----------------------------|
| `INIT`  | EInit | Service Initialization     |

### **Event Outputs**
| Event | Type   | With Variables         | Comment                        |
|-------|--------|------------------------|--------------------------------|
| `INITO` | EInit | –                      | Initialization Confirm        |
| `CNF`  | Event  | `STATUS`, `s16result`  | Confirmation of Requested Service |

### **Data Inputs
*None – all data is exchanged via the adapters.*

### **Data Outputs**
| Data Name  | Type   | Comment |
|------------|--------|---------|
| `STATUS`   | STRING | Service Status |
| `s16result`| INT    | Retval – see description in the core block |

### **Adapters**
| Direction | Name          | Type                                | Comment              |
|-----------|---------------|-------------------------------------|----------------------|
| Socket    | `u16NewMaskId`| `adapter::types::unidirectional::AUI` | New active mask ID   |
| Plug      | `u16OldMaskId`| `adapter::types::unidirectional::AUI` | Old active mask ID   |

The adapter data (D1) carries a 16‑bit unsigned integer (UINT) representing the mask ID. The AUI adapter provides a single event (E1) on both sides to trigger the transfer.

## Functionality

The block acts as a bridge between an AUI adapter network and the internal `Q_ActiveMask` function block. When the **u16NewMaskId** socket receives a new value via its `E1` event, the wrapper internally issues a `REQ` to the `Q_ActiveMask` block, supplying the new mask ID. After processing, the `Q_ActiveMask` block produces a `CNF` event, which is propagated to the output as `CNF`, together with the `STATUS` and `s16result` values. Additionally, the old mask ID is sent back through the **u16OldMaskId** plug using its `E1` event, and the corresponding data value is transferred.

Initialization is handled by the `INIT` event, which is forwarded to the internal block, and `INITO` is generated once the initialization completes.

This wrapper does not implement the service logic itself; it relies entirely on the `Q_ActiveMask` function block for the actual ISO 11783‑6 command (Part 6, F.34). The wrapper only adapts the data flow to and from the AUI interfaces.

## Technical Features

- **AUI Adapter Compatible**: Uses the standard unidirectional AUI adapter type, allowing direct connection to other adapter‑based components.
- **Seamless Integration**: The internal `Q_ActiveMask` block is fully encapsulated; only the adapter interface is exposed.
- **Error and Status Reporting**: Provides textual status (`STATUS`) and a numeric result (`s16result`) on every confirmation.
- **Event‑Driven**: All operations are triggered by incoming events (init, new mask ID), making it suitable for cyclic or event‑driven control systems.
- **Platform Independent**: The block is written in standard IEC 61499 and can be used in any 4diac‑compatible runtime.

## State Overview

The block does not maintain an explicit state machine of its own; it inherits the behaviour of the wrapped `Q_ActiveMask` block. The internal block typically operates in the following states:

1. **Idle**: Waiting for a new mask ID command.
2. **Busy**: Processing a request (between `REQ` and `CNF`).
3. **Completed**: A `CNF` has been issued, the block returns to idle.

The wrapper only forwards events and data accordingly. The `INIT` event initialises the internal block, and `INITO` confirms successful initialisation.

## Application Scenarios

- **ISOBUS Virtual Terminal Control**: Use this block in agricultural machinery to change the currently displayed working set or main mask on a VT.
- **Adapter‑Based Architectures**: In systems where all external communication is performed via AUI adapters, this wrapper eliminates the need to expose raw event/data ports.
- **Multi‑Mask Management**: Combine multiple instances to manage several mask IDs or to switch between different screens based on operational state.
- **Prototyping and Simulation**: The wrapper simplifies testing by providing a clean adapter interface that can be easily driven by simulation models or other adapters.

## Comparison with Similar Blocks

- **Q_ActiveMask (core block)**: The core block provides separate event and data inputs/outputs. The AUI wrapper abstracts those into a single adapter pair, reducing wiring complexity, but adds an extra layer of indirection.
- **Q_ActiveMask_AUI vs. other adapter wrappers**: Many ISOBUS commands have similar AUI wrappers (e.g., `Q_SetBackgroundColor_AUI`). They all follow the same pattern – a socket for the “new” data, a plug for the “old” data, and common event outputs.

The main advantage of this wrapper is that it fits directly into adapter‑based communication frameworks, promoting modular design and reusability. The disadvantage is a slight increase in block count and the need to manage two adapter connections.

## Conclusion

The **Q_ActiveMask_AUI** function block provides a pragmatic and clean solution for changing the active mask in an ISOBUS VT using unidirectional AUI adapters. It encapsulates the underlying `Q_ActiveMask` logic, exposes a minimal and well‑defined interface, and integrates smoothly into event‑driven industrial control systems. Its use of standard AUI types ensures high compatibility with existing adapter‑oriented designs, making it a valuable component for ISOBUS applications.
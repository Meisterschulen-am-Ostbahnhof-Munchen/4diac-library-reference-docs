# Q_ESC_AX

![Q_ESC_AX](./Q_ESC_AX.svg)

* * * * * * * * * *

## Introduction

The **Q_ESC_AX** function block is an AX adapter wrapper for **Q_ESC** (ISO 11783‑6, Part 6 – F.8). It enables triggering the ESC command (aborting operator input on the VT) using a unidirectional AX event adapter interface.

## Interface Structure

### **Event Inputs**

| Name | Type | Comment |
|---|---|---|
| `INIT` | `EInit` | Service Initialization |

### **Event Outputs**

| Name | Type | Comment |
|---|---|---|
| `INITO` | `EInit` | Initialization Confirm |
| `CNF` | `Event` | Confirmation of Requested Service |

### **Data Inputs**

No data inputs required — triggered via AX adapter socket.

### **Data Outputs**

| Name | Type | Comment |
|---|---|---|
| `STATUS` | `STRING` | Service Status |
| `s16result` | `INT` | Command return code |

### **Adapters**

| Type | Name | Direction | Comment |
|---|---|---|---|
| `adapter::types::unidirectional::AX` | `xReq` | Socket (Input) | Trigger for ESC command |

## Functionality

Connects inner FB `Q_ESC` (*isobus::UT::Q::Q_ESC*) to AX adapter `xReq`.
An event on `xReq.E1` triggers `REQ` on the inner FB, sending the ESC command to the VT. Execution status is returned via `CNF`, `STATUS`, and `s16result`.

## Technical Features

- **Event-driven Abort:** Allows aborting VT input dialogs via AX adapter events.
- **ISOBUS Standard Compliance:** Complies with ISO 11783-6 F.8.

## State Overview

Delegates state handling to inner `Q_ESC`.

## Application Scenarios

- Aborting input dialogs via buttons, error conditions, or sequence logic over AX adapter lines.

## Comparison with Similar Blocks

Wraps `Q_ESC` with an `AX` adapter socket for seamless integration into adapter networks.

## Conclusion

**Q_ESC_AX** provides an event-driven AX adapter wrapper for executing the ISOBUS ESC command.

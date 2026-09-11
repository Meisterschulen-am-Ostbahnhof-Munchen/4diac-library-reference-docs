# Q_ExecuteMacro_AUI

![Q_ExecuteMacro_AUI](./Q_ExecuteMacro_AUI.svg)

* * * * * * * * * *

## Introduction

The **Q_ExecuteMacro_AUI** function block is an AUI adapter wrapper for **Q_ExecuteMacro** (ISO 11783‑6, Part 6 – F.48). It enables executing macros on an ISOBUS Virtual Terminal using a unidirectional AUI adapter interface (UINT). The block encapsulates the internal `Q_ExecuteMacro` core and provides a clean, adapter-based interface.

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

No direct data inputs — macro Object ID is received via the AUI adapter socket.

### **Data Outputs**

| Name | Type | Comment |
|---|---|---|
| `STATUS` | `STRING` | Service Status |
| `s16result` | `INT` | Command return code |

### **Adapters**

| Type | Name | Direction | Comment |
|---|---|---|---|
| `adapter::types::unidirectional::AUI` | `u16ObjId` | Socket (Input) | Object ID of macro to execute (1–255) |

## Functionality

Internally connects `Q_ExecuteMacro` (*isobus::UT::Q::Q_ExecuteMacro*) to the AUI adapter interface.
Upon receiving an event at socket `u16ObjId.E1`, the internal FB's `REQ` event is triggered with the macro ID from `u16ObjId.D1`. Following execution, the internal FB emits `CNF` and updates `STATUS` and `s16result`.

## Technical Features

- **Adapter Wrapper:** Input data received via AUI socket.
- **8-bit Macro Range:** Handles macro IDs in the range 1–255.
- **ISOBUS Standard Compliance:** Complies with ISO 11783‑6 Annex F.48.

## State Overview

Delegates all state handling to the inner FB `Q_ExecuteMacro`.

1. **Initialization** – `INIT` initializes the inner FB and confirms with `INITO`.
2. **Command Execution** – Event on `u16ObjId.E1` triggers macro execution.
3. **Confirmation** – `CNF` confirms completion with updated `STATUS` and `s16result`.

## Application Scenarios

- Triggering VT macros via AUI adapter in ISO 11783-6 applications.
- Sequence-driven macro execution.

## Comparison with Similar Blocks

Compared to direct `Q_ExecuteMacro`, **Q_ExecuteMacro_AUI** provides an adapter interface. For macro IDs > 255, use **Q_ExecuteExtendedMacro_AUI**.

## Conclusion

**Q_ExecuteMacro_AUI** is a clean adapter wrapper for executing ISOBUS VT macros via AUI adapter connections.

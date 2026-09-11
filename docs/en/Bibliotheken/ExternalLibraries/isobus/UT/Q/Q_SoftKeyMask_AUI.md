# Q_SoftKeyMask_AUI

![Q_SoftKeyMask_AUI](./Q_SoftKeyMask_AUI.svg)

* * * * * * * * * *

## Introduction

The **Q_SoftKeyMask_AUI** function block is an AUI adapter wrapper for **Q_SoftKeyMask** (ISO 11783‑6, Part 6 – F.36). It enables switching the active soft key mask on an ISOBUS Virtual Terminal using AUI adapter connections.

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

| Name | Type | Comment |
|---|---|---|
| `u8MaskType` | `USINT` | Mask Type (0: DataMask, 1: AlarmMask) |
| `u16DataMaskId` | `UINT` | Object ID of target Data/AlarmMask |

### **Data Outputs**

| Name | Type | Comment |
|---|---|---|
| `STATUS` | `STRING` | Service Status |
| `s16result` | `INT` | Command return code |

### **Adapters**

| Type | Name | Direction | Comment |
|---|---|---|---|
| `adapter::types::unidirectional::AUI` | `u16SoftKeyMaskId` | Socket (Input) | New soft key mask Object ID |
| `adapter::types::unidirectional::AUI` | `u16OldSoftKeyMaskId` | Plug (Output) | Previous soft key mask Object ID |

## Functionality

Encapsulates inner FB `Q_SoftKeyMask` (*isobus::UT::Q::Q_SoftKeyMask*).
An event on `u16SoftKeyMaskId.E1` triggers `REQ` on the inner FB, which switches the soft key mask. Upon completion, the previous soft key mask ID is output via `u16OldSoftKeyMaskId.D1` and `u16OldSoftKeyMaskId.E1`.

## Technical Features

- **Adapter-driven Softkey Control:** Flexible switching of soft key layouts via AUI adapters.
- **ISOBUS Standard Compliance:** Complies with ISO 11783-6 F.36.

## State Overview

Delegates state processing to inner `Q_SoftKeyMask`.

## Application Scenarios

- Dynamic soft key layout switching in ISOBUS VT terminals via AUI adapters.

## Comparison with Similar Blocks

Wraps `Q_SoftKeyMask` with an AUI adapter interface for integration into adapter networks.

## Conclusion

**Q_SoftKeyMask_AUI** is the adapter wrapper for soft key mask switching in ISOBUS VT applications.

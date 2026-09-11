# Q_SelectColourMap_AUI

![Q_SelectColourMap_AUI](./Q_SelectColourMap_AUI.svg)

* * * * * * * * * *

## Introduction

The **Q_SelectColourMap_AUI** function block is an AUI adapter wrapper for **Q_SelectColourMap** (ISO 11783‑6, Part 6 – F.60). It enables selecting the active Colour Map / palette on an ISOBUS Virtual Terminal using AUI adapter connections.

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

No direct data inputs — Colour Map IDs are handled via AUI adapters.

### **Data Outputs**

| Name | Type | Comment |
|---|---|---|
| `STATUS` | `STRING` | Service Status |
| `s16result` | `INT` | Command return code |

### **Adapters**

| Type | Name | Direction | Comment |
|---|---|---|---|
| `adapter::types::unidirectional::AUI` | `u16ObjIdColourMap` | Socket (Input) | New Colour Map Object ID |
| `adapter::types::unidirectional::AUI` | `u16OldObjIdColourMap` | Plug (Output) | Previous Colour Map Object ID |

## Functionality

Encapsulates inner FB `Q_SelectColourMap` (*isobus::UT::Q::Q_SelectColourMap*).
An event on `u16ObjIdColourMap.E1` triggers `REQ` on the inner FB with the ID from `u16ObjIdColourMap.D1`. Upon completion, the previous ID is output via `u16OldObjIdColourMap.D1` and `CNF`.

## Technical Features

- **AUI Adapter Integration:** Full adapter-driven selection of Colour Maps.
- **ISOBUS Standard Compliance:** Complies with ISO 11783-6 F.60.

## State Overview

Delegates state processing to inner `Q_SelectColourMap`.

## Application Scenarios

- Dynamic colour scheme switching (e.g. day/night mode) on ISOBUS VT terminals via AUI adapters.

## Comparison with Similar Blocks

Wraps `Q_SelectColourMap` with AUI adapter socket and plug for seamless integration into adapter networks.

## Conclusion

**Q_SelectColourMap_AUI** is the adapter wrapper for Colour Map selection in ISOBUS VT applications.

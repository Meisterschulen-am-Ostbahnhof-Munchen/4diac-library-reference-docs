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
| `adapter::types::unidirectional::AUI` | `u16ObjIdColourMap` | Socket (Input) | New Colour Map Object ID (39000–39999 for ColourMap, 45000–45999 for ColourPalette, ID_NULL / 0xFFFF for default table) |
| `adapter::types::unidirectional::AUI` | `u16OldObjIdColourMap` | Plug (Output) | Previous Colour Map Object ID |

## Functionality

Encapsulates inner FB `Q_SelectColourMap` (*isobus::UT::Q::Q_SelectColourMap*).
An event on `u16ObjIdColourMap.E1` triggers `REQ` on the inner FB with the ID from `u16ObjIdColourMap.D1`. Upon completion, the previous ID is output via `u16OldObjIdColourMap.D1` and `CNF`.

## Technical Features

- **AUI Adapter Integration:** Adapter-driven selection of Colour Maps via `u16ObjIdColourMap` socket.
- **ID Ranges per ISO 11783-6 F.60:**
  - `ColourMap`: 39000–39999 (VT version 4 and later)
  - `ColourPalette`: 45000–45999 (VT version 6 and later)
  - `ID_NULL` (`0xFFFF` / 65535): Restores the default colour table.
- **Implementation Limitations (VTClientHelper):**
  - `VTClientHelper::iso_is_colour_map_id` currently only accepts the range 39000–39999 (`ColourPalette` IDs 45000–45999 are rejected).
  - `cmd_select_colour_map_or_palette` consumes `ID_NULL` (`0xFFFF`), returns `VT_E_NO_ERR` (0), and sends no VT command. Restoring the default colour table is therefore currently not supported.

## State Overview

Delegates state processing to inner `Q_SelectColourMap`.

## Application Scenarios

- Dynamic colour scheme switching (e.g. day/night mode) on ISOBUS VT terminals via AUI adapters.

## Comparison with Similar Blocks

Wraps `Q_SelectColourMap` with AUI adapter socket and plug for seamless integration into adapter networks.

## Conclusion

**Q_SelectColourMap_AUI** is the adapter wrapper for Colour Map selection in ISOBUS VT applications.

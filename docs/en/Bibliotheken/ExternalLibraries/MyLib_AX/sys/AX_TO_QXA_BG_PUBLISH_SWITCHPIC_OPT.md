# AX_TO_QXA_BG_PUBLISH_SWITCHPIC_OPT

![AX_TO_QXA_BG_PUBLISH_SWITCHPIC_OPT_network](./AX_TO_QXA_BG_PUBLISH_SWITCHPIC_OPT_network.svg)

* * * * * * * * * *

## Introduction

`AX_TO_QXA_BG_PUBLISH_SWITCHPIC_OPT` corresponds to [`AX_TO_QXA_BG_PUBLISH_SWITCHPIC`](./AX_TO_QXA_BG_PUBLISH_SWITCHPIC.md), but has two `AOption` sockets instead of `QI`/`INIT`: `AOption_Output` switches only the physical output, `AOption_Anzeige` switches background, publish and ObjectPointer. `INIT_CNF` is returned for real in each case. The block is a copy of `AX_TO_QXA_BG_PUBLISH_SWITCHPIC` v1.2; the reason was that with the PVEA variant the SoftKey background, ObjectPointer and state publish stayed inactive. The output part is also available on its own as [`AX_TO_QXA_Option`](./AX_TO_QXA_Option.md), the display part as [`AX_TO_BG_PUBLISH_SWITCHPIC_Option`](./AX_TO_BG_PUBLISH_SWITCHPIC_Option.md).

![AX_TO_QXA_BG_PUBLISH_SWITCHPIC_OPT](AX_TO_QXA_BG_PUBLISH_SWITCHPIC_OPT.svg)

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

| Variable | Type | Default | Comment |
|----------|------|---------|---------|
| `u16ObjId` | UINT | `ID_NULL` | Object ID SoftKey; carries the background colour. |
| `u16ObjIdA` | UINT | `ID_NULL` | Object ID AuxFunction2 (joystick); also shows the background colour on the Aux handle. |
| `Output` | `logiBUS::io::DQ::logiBUS_DO_S` | `logiBUS_DO::Invalid` | Selection of the output `Output_Q1`..`Output_Q12`. |
| `ID_STATE_WRITE` | WSTRING |  | Local publish address (`ACTION=WRITE`) for the state; subscribed remotely by web client/dashboard. |
| `Picture` | UINT |  | Object ID of the ObjectPointer carrying the status box content. |
| `pictures` | `isobus::UT::Q::types::s2x1ObjectIDs` | `(up := (u16ObjId := ID_NULL), down := (u16ObjId := ID_NULL))` | The two target object IDs (`up` = off, `down` = on). |

### **Data Outputs**

| Variable | Type | Default | Comment |
|----------|------|---------|---------|
| `QO` | BOOL |  | Quality output display (AND of `Publish.QO` and `ObjPtr.QO`). |
| `QO_Output` | BOOL |  | Quality output physical output (`Publish.QO_Output`). |

### **Adapters**

| Adapter | Type | Direction | Comment |
|---------|------|-----------|---------|
| `IN` | `adapter::types::unidirectional::AX` | Socket | Finished AX state; distributed to output, background, publish and ObjectPointer status display. |
| `AOption_Output` | `Option::AOption` | Socket | Enables only the physical output (e.g. DEMUX branch classic). |
| `AOption_Anzeige` | `Option::AOption` | Socket | Enables background, publish and ObjectPointer (e.g. `AOption_OR_2` of classic and PVEA). |

## Function Blocks (FBs) Used

- **AX_SPLIT_2** (`adapter::events::unidirectional::AX_SPLIT_2`): adapter fan-out (strictly 1:1)
- **AOption_X_TO_EVENT_Output** (`Option::conversion::AOption_X_TO_EVENT`): option for the physical output
- **AOption_X_TO_EVENT_Anzeige** (`Option::conversion::AOption_X_TO_EVENT`): option for the display
- **Publish** (SubApp `MyLib::sys::AX_TO_QXA_BG_PUBLISH_OPT`): output, background and state publish; see [`AX_TO_QXA_BG_PUBLISH_OPT`](./AX_TO_QXA_BG_PUBLISH_OPT.md)
- **ObjPtr** (SubApp `MyLib::sys::AX_SwitchPic_2_1`): ObjectPointer status display; see [`AX_SwitchPic_2_1`](./AX_SwitchPic_2_1.md)
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_2`): combines the `QO` of the display

## Program Flow and Connections

1. AX state: `IN` -> `AX_SPLIT_2.IN`; `OUT1` -> `Publish.IN`; `OUT2` -> `ObjPtr.DI1`.
2. Options: `AOption_Output` -> `AOption_X_TO_EVENT_Output.AOption_IN`; `AOption_Anzeige` -> `AOption_X_TO_EVENT_Anzeige.AOption_IN`.
3. Display initialization: `AOption_X_TO_EVENT_Anzeige.INIT_REQ` -> `Publish.INIT`; `Publish.INITO` -> `ObjPtr.INIT`; `ObjPtr.INITO` -> `AOption_X_TO_EVENT_Anzeige.INIT_CNF` and -> `AND_QO.REQ`.
4. Output initialization: `AOption_X_TO_EVENT_Output.INIT_REQ` -> `Publish.INIT_Output`; `Publish.INITO_Output` -> `AOption_X_TO_EVENT_Output.INIT_CNF`.
5. Qualifier: `AOption_X_TO_EVENT_Anzeige.QI_OUT` -> `Publish.QI` and `ObjPtr.QI`; `AOption_X_TO_EVENT_Output.QI_OUT` -> `Publish.QI_Output`.
6. Quality: `Publish.QO` -> `AND_QO.IN1`; `ObjPtr.QO` -> `AND_QO.IN2`; `AND_QO.OUT` -> `QO`; `Publish.QO_Output` -> `QO_Output`.
7. Data: `u16ObjId`, `u16ObjIdA`, `Output`, `ID_STATE_WRITE` -> inputs of `Publish` with the same name; `Picture` -> `ObjPtr.Picture`; `pictures` -> `ObjPtr.pictures`.

## Technical Features

- The block has no event inputs/outputs of its own; initialization runs via the two `AOption` sockets.
- Output and display can be enabled independently of each other.
- The `AX_SPLIT_2` is needed because the adapter fan-out is strictly 1:1.

## Application Scenarios

- Several identically structured channels (e.g. conveyor belts) with output, SoftKey colour, publish and status picture, where the output should be active for one option only (e.g. classic), but the display for several options (e.g. classic and PVEA).

## Summary

`AX_TO_QXA_BG_PUBLISH_SWITCHPIC_OPT` distributes a finished AX state to output, background, publish and ObjectPointer status display and enables output and display via two separate `AOption` sockets.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

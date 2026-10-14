# RedWhiteBackground2_AXS

![RedWhiteBackground2_AXS_network](./RedWhiteBackground2_AXS_network.svg)

* * * * * * * * * *

## Introduction

`RedWhiteBackground2_AXS` switches the VT background color of 2 objects based on a boolean selector signal: `TRUE` → **Rot**, `FALSE` → **Weiß**. The selector signal arrives via an `AX` adapter socket (`DI1`). The object ID is passed via the structured type `s1ObjectID` (`u16ObjIds`).

| Position | Object ID source       | Block                                               |
| -------- | ---------------------- | --------------------------------------------------- |
| 1        | `F_MOVE.OUT.u16ObjIdA` | `Q_BackgroundColour` (regular object)               |
| 2        | `F_MOVE.OUT.u16ObjIdA` | `Q_BackgroundColourAux` (auxiliary function object) |

For the general pattern (selector → `AX_SEL`/`F_SEL` → `Q_BackgroundColour`), see [Background Color Blocks (shared pattern)](./Background-Color-Blocks.md).

![RedWhiteBackground2_AXS](RedWhiteBackground2_AXS.svg)

## Initialization and Status (INIT/INITO, QI/QO)

- **Event input** `INIT` (`EInit`, Initialization Request) and **event output** `INITO` (`EInit`, Initialization Confirm)
- **Data input** `QI` (`BOOL`, default `TRUE`, Quality Input) is forwarded to `Q_BackgroundColour_1.QI` and `Q_BackgroundColour_2.QI`
- **Data output** `QO` (`BOOL`, Quality Output): AND (`AND_QO`, type `iec61131::booleanOperators::AND_BOOL_2`) of the `QO` outputs of `Q_BackgroundColour_1.QO` and `Q_BackgroundColour_2.QO`
- **Initialization chain**: `INIT` → `Q_BackgroundColour_1.INIT`, `Q_BackgroundColour_1.INITO` → `Q_BackgroundColour_2.INIT`, `Q_BackgroundColour_2.INITO` → `INITO`
- `E_MERGE_QO` (type `iec61499::events::E_MERGE_3`) merges `Q_BackgroundColour_2.INITO`, `Q_BackgroundColour_1.CNF` and `Q_BackgroundColour_2.CNF` and triggers `AND_QO.REQ`

## Summary

One of many variants in the background color block family: color pair Rot/Weiß, 2 objects, adapter selector, struct. object ID.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

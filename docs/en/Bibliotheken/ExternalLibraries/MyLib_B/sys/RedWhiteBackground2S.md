# RedWhiteBackground2S

![RedWhiteBackground2S_network](./RedWhiteBackground2S_network.svg)

* * * * * * * * * *

## Introduction

`RedWhiteBackground2S` switches the VT background color of 2 objects based on a boolean selector signal: `TRUE` → **Rot**, `FALSE` → **Weiß**. The selector signal arrives as a plain `BOOL` data input (`DI1`). The object ID is passed via the structured type `s2ObjectIDs` (`u16ObjIds`).

| Position | Object ID source       | Block                                               |
| -------- | ---------------------- | --------------------------------------------------- |
| 1        | `F_MOVE.OUT.u16ObjIdA` | `Q_BackgroundColour` (regular object)               |
| 2        | `F_MOVE.OUT.u16ObjIdA` | `Q_BackgroundColourAux` (auxiliary function object) |

For the general pattern (selector → `AX_SEL`/`F_SEL` → `Q_BackgroundColour`), see [Background Color Blocks (shared pattern)](../../MyLib_AX/sys/Background-Color-Blocks.md).

![RedWhiteBackground2S](RedWhiteBackground2S.svg)

## Initialization and Quality

- **Event input `INIT`** (`EInit`) and **event output `INITO`** (`EInit`): initialization is chained through the `Q_BackgroundColour` instances: `INIT` → `Q_BackgroundColour_2.INIT`; `Q_BackgroundColour_2.INITO` → `Q_BackgroundColour_3.INIT`; `Q_BackgroundColour_3.INITO` → `INITO`.
- **Data input `QI`** (`BOOL`, default `TRUE`) is distributed to `.QI` of all instances (`Q_BackgroundColour_2`, `Q_BackgroundColour_3`).
- **Data output `QO`** (`BOOL`): AND of all instances via `AND_QO` (`iec61131::booleanOperators::AND_BOOL_2`): `Q_BackgroundColour_2.QO` → `AND_QO.IN1`, `Q_BackgroundColour_3.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`.
- `E_MERGE_QO` (`iec61499::events::E_MERGE_3`) merges `Q_BackgroundColour_3.INITO` and the `CNF` events of both instances (`Q_BackgroundColour_2.CNF`, `Q_BackgroundColour_3.CNF`) and thereby triggers `AND_QO.REQ`.

## Summary

One of many variants in the background color block family: color pair Rot/Weiß, 2 objects, BOOL selector, struct. object ID.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

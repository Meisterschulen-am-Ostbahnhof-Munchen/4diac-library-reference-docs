# GreenRedBackground2S

![GreenRedBackground2S_network](./GreenRedBackground2S_network.svg)

* * * * * * * * * *

## Introduction

`GreenRedBackground2S` switches the VT background color of 2 objects based on a boolean selector signal: `TRUE` → **Grün**, `FALSE` → **Rot**. The selector signal arrives as a plain `BOOL` data input (`DI1`). The object ID is passed via the structured type `s1ObjectID` (`u16ObjIds`).

| Position | Object ID source       | Block                                               |
| -------- | ---------------------- | --------------------------------------------------- |
| 1        | `F_MOVE.OUT.u16ObjIdA` | `Q_BackgroundColour` (regular object)               |
| 2        | `F_MOVE.OUT.u16ObjIdA` | `Q_BackgroundColourAux` (auxiliary function object) |

Interface (quality pattern): event input `INIT` and event output `INITO` (each `EInit`), data input `QI` (`BOOL`, default `TRUE`) and data output `QO` (`BOOL`, AND of all instances). Initialization: `INIT` → `Q_BackgroundColour_1.INIT`; `Q_BackgroundColour_1.INITO` → `Q_BackgroundColour_2.INIT`; `Q_BackgroundColour_2.INITO` → `INITO`. Quality: `QI` → `Q_BackgroundColour_1.QI`, `Q_BackgroundColour_2.QI`; `Q_BackgroundColour_1.QO`, `Q_BackgroundColour_2.QO` → `AND_QO.IN1` to `AND_QO.IN2` (`AND_QO`: `iec61131::booleanOperators::AND_BOOL_2`); `AND_QO.OUT` → `QO`. The trigger `AND_QO.REQ` comes via `E_MERGE_QO` (`E_MERGE_3`), which merges `Q_BackgroundColour_2.INITO` and the `CNF` of `Q_BackgroundColour_1` and `_2`; `E_MERGE_QO.EO` → `AND_QO.REQ`.

For the general pattern (selector → `AX_SEL`/`F_SEL` → `Q_BackgroundColour`), see [Background Color Blocks (shared pattern)](../../MyLib_AX/sys/Background-Color-Blocks.md).

![GreenRedBackground2S](GreenRedBackground2S.svg)

## Summary

One of many variants in the background color block family: color pair Grün/Rot, 2 objects, BOOL selector, struct. object ID.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

# GreenRedBackground2_AXS

![GreenRedBackground2_AXS_network](./GreenRedBackground2_AXS_network.svg)

* * * * * * * * * *

## Introduction

`GreenRedBackground2_AXS` switches the VT background color of 2 objects based on a boolean selector signal: `TRUE` → **Grün**, `FALSE` → **Rot**. The selector signal arrives via an `AX` adapter socket (`DI1`). The object ID is passed via the structured type `s2ObjectIDs` (`u16ObjIds`).

| Position | Object ID source       | Block                                               |
| -------- | ---------------------- | --------------------------------------------------- |
| 1        | `F_MOVE.OUT.u16ObjIdA` | `Q_BackgroundColour` (regular object)               |
| 2        | `F_MOVE.OUT.u16ObjIdA` | `Q_BackgroundColourAux` (auxiliary function object) |

For the general pattern (selector → `AX_SEL`/`F_SEL` → `Q_BackgroundColour`), see [Background Color Blocks (shared pattern)](./Background-Color-Blocks.md).

**Initialization and quality:** The SubApp has `INIT`/`INITO` and `QI`/`QO`. `INIT` → `Q_BackgroundColour_1.INIT`, `Q_BackgroundColour_1.INITO` → `Q_BackgroundColour_2.INIT`, `Q_BackgroundColour_2.INITO` → `INITO`. `QI` goes to both `Q_BackgroundColour` instances. `QO` is the `AND` (`AND_QO`, `AND_BOOL_2`) of the two `QO` outputs; `AND_QO.REQ` is triggered via `E_MERGE_QO` (`E_MERGE_3`: `Q_BackgroundColour_2.INITO`, `Q_BackgroundColour_1.CNF`, `Q_BackgroundColour_2.CNF`).

![GreenRedBackground2_AXS](GreenRedBackground2_AXS.svg)

## Summary

One of many variants in the background color block family: color pair Grün/Rot, 2 objects, adapter selector, struct. object ID.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

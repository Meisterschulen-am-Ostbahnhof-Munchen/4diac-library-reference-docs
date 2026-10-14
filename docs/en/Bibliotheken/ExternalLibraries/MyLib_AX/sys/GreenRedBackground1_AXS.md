# GreenRedBackground1_AXS

![GreenRedBackground1_AXS_network](./GreenRedBackground1_AXS_network.svg)

* * * * * * * * * *

## Introduction

`GreenRedBackground1_AXS` switches the VT background color of one object based on a boolean selector signal: `TRUE` → **Grün**, `FALSE` → **Rot**. The selector signal arrives via an `AX` adapter socket (`DI1`). The object ID is passed via the structured type `s1ObjectID` (`u16ObjIds`).

For the general pattern (selector → `AX_SEL`/`F_SEL` → `Q_BackgroundColour`), see [Background Color Blocks (shared pattern)](./Background-Color-Blocks.md).

Quality pattern: the SubApp has the event input `INIT`, the event output `INITO`, the data input `QI` (`BOOL`, default `TRUE`, "Quality Input - TRUE to enable service") and the data output `QO` (`BOOL`, "Quality Output - TRUE if service executed successfully"). They are passed straight through to `Q_BackgroundColour`: `INIT` → `Q_BackgroundColour.INIT`, `Q_BackgroundColour.INITO` → `INITO`, `QI` → `Q_BackgroundColour.QI`, `Q_BackgroundColour.QO` → `QO`.

![GreenRedBackground1_AXS](GreenRedBackground1_AXS.svg)

## Summary

One of many variants in the background color block family: color pair Grün/Rot, 1 object, adapter selector, struct. object ID.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

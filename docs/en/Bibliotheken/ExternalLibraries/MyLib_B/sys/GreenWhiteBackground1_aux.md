# GreenWhiteBackground1_aux

![GreenWhiteBackground1_aux_network](./GreenWhiteBackground1_aux_network.svg)

* * * * * * * * * *

## Introduction

`GreenWhiteBackground1_aux` switches the VT background color of one object based on a boolean selector signal: `TRUE` → **Grün**, `FALSE` → **Weiß**. The selector signal arrives as a plain `BOOL` data input (`DI1`). The object ID is passed via input `u16ObjId`.

For the general pattern (selector → `AX_SEL`/`F_SEL` → `Q_BackgroundColour`), see [Background Color Blocks (shared pattern)](../../MyLib_AX/sys/Background-Color-Blocks.md).

**Quality ports:** The block additionally has the event `INIT` (input) and `INITO` (output), the data input `QI` (BOOL, default `TRUE`, "Quality Input - TRUE to enable service") and the data output `QO` (BOOL, "Quality Output - TRUE if service executed successfully"). `INIT` is passed on to `Q_BackgroundColourAux.INIT`, `Q_BackgroundColourAux.INITO` is passed to `INITO`; `QI` is connected to `Q_BackgroundColourAux.QI` and `Q_BackgroundColourAux.QO` to `QO`.

![GreenWhiteBackground1_aux](GreenWhiteBackground1_aux.svg)

## Summary

One of many variants in the background color block family: color pair Grün/Weiß, 1 object, BOOL selector.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

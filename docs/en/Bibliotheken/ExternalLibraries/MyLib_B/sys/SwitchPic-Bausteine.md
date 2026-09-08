# SwitchPic(Col) Building Blocks: Common Pattern

* * * * * * * * * *

## Introduction

`MyLib::sys` (test_B) contains a family of building blocks that **display a different VT image (object pointer target) and/or a different background color depending on the state** — e.g., a slider/valve animation with the states Unknown/Closed/Opening/Opened/Closing. This page explains the common pattern; the individual pages only list the specific variations.

## Naming Scheme

`SwitchPic[Col]_<Zustände>_<Variante>[_aux]`

| Component | Meaning |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`Col`** (optional) | In addition to changing the image, also toggles a background color (`Q_BackgroundColour`) to match the state, not just the image (`Q_NumericValue`). |
| **States** (`2` or `5`) | `2` = Boolean selector `DI1` (`up`/`down`, type `s2x1ObjectIDs`/`s2x2ObjectIDs`); `5` = `iSTATE`-Selector (`USINT`, slider state machine Unknown/Closed/Opening/Opened/Closing, type `SchieberStruct`/`SchieberAuxInStruct`), evaluated via `F_MUX_5`. |
| **Variant** (`1`/`2`/`3`, only with `SwitchPic`) | Number/Type of VT objects updated simultaneously: `1` = only a normal VT object (Softkey/DataMask), `2` = additionally an AUX object, `3` = additionally an AUX object AND a second normal object ("Button"). |
| **`_aux`** | Only AUX object(s) are switched, no normal VT object (counterpart to variant `1`, but exclusively for Auxiliary Function objects). |


## Functionality

1. A structure `pictures`/`Sets` (type depends on state/variant) contains the corresponding object ID(s) (image and, if applicable, color) for each possible state.

2. `F_MOVE` unpacks this structure into individual values.

3. A multiplexer (`F_SEL` for 2 states, `F_MUX_5` for 5 states, controlled by `DI1` or `iSTATE`) selects the values matching the current state.

4. `Q_NumericValue`/`Q_NumericValueAux` sets the selected object pointer value to the VT object identified by `Picture`/`PictureA`/`PictureB`; for `Col` variants, `Q_BackgroundColour`/`Q_BackgroundColourAux` additionally sets the background color to the object identified by `Color`.

5. For multiple target objects (variant 2/3), several `Q_NumericValue(Aux)` instances are chained together in a fixed INIT sequence (each triggers the next via `INITO`) before `INITO` is reported externally.

## Summary

A multiplexer pattern: State → Structure lookup (`F_MOVE`) → Selection (`F_SEL`/`F_MUX_5`) → one or more `Q_NumericValue(Aux)`/`Q_BackgroundColour(Aux)` targets. The variant number (1/2/3) and `Col`/`_aux` determine only how many targets of which type (normal/AUX, image/color) are updated simultaneously.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

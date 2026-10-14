# SwitchPicCol_5_1

![SwitchPicCol_5_1_network](./SwitchPicCol_5_1_network.svg)

* * * * * * * * * *

## Introduction

`SwitchPicCol_5_1` switches both a VT picture (`Picture`, via `Q_NumericValue`) **and** a VT background color (`Color`, via `Q_BackgroundColour`) between 5 states (slide-valve animation) — two parallel `F_MUX_5` multiplexers (one for picture IDs, one for color values), both driven by the same `iSTATE` value, targeting regular (non-AUX) objects.

For the general pattern, see [SwitchPic(Col) Blocks (shared pattern)](./SwitchPic-Blocks.md).

**Initialization and quality:** `INIT` → `F_MOVE.REQ`; `F_MOVE.CNF` → `PictureVT.INIT`; `PictureVT.INITO` → `ColorVT.INIT`; `ColorVT.INITO` → `INITO`. New are `QI` (`BOOL`, default `TRUE`), which goes to `PictureVT.QI` and `ColorVT.QI`, and `QO` (`BOOL`): `AND` (`AND_QO`, `AND_BOOL_2`) of `PictureVT.QO` and `ColorVT.QO`. `AND_QO.REQ` is triggered via `E_MERGE_QO` (`E_MERGE_3`: `ColorVT.INITO`, `PictureVT.CNF`, `ColorVT.CNF`).

![SwitchPicCol_5_1](SwitchPicCol_5_1.svg)

## Summary

Combines picture and color switching (`Col`) for 5 states on regular VT objects.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

# SwitchPic_2_1

![SwitchPic_2_1_network](./SwitchPic_2_1_network.svg)

* * * * * * * * * *

## Introduction

`SwitchPic_2_1` switches a VT picture (object pointer) between 2 states (`up`/`down`) based on a boolean selector (`DI1`) — only the regular VT object (`Picture`, via `Q_NumericValue`), no AUX object.

For the general pattern, see [SwitchPic(Col) Blocks (shared pattern)](./SwitchPic-Blocks.md).

![SwitchPic_2_1](SwitchPic_2_1.svg)

## Interface (QI/QO)

The block has the data input `QI` (`BOOL`, default `TRUE`, Quality Input) and the data output `QO` (`BOOL`, Quality Output). `QI` is passed through directly to `Q_NumericValue.QI`, and `Q_NumericValue.QO` directly to `QO`.

## Summary

Variant "1" (regular object only) of the 2-state picture switch.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

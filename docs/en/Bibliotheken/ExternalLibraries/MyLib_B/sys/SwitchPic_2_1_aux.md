# SwitchPic_2_1_aux

![SwitchPic_2_1_aux_network](./SwitchPic_2_1_aux_network.svg)

* * * * * * * * * *

## Introduction

`SwitchPic_2_1_aux` switches a VT picture between 2 states (`up`/`down`) based on a boolean selector (`DI1`) — exclusively for an **auxiliary function** object (`Q_NumericValueAux`), no regular VT object.

For the general pattern, see [SwitchPic(Col) Blocks (shared pattern)](./SwitchPic-Blocks.md).

![SwitchPic_2_1_aux](SwitchPic_2_1_aux.svg)

## Interface (QI/QO)

The block has the data input `QI` (`BOOL`, default `TRUE`, Quality Input) and the data output `QO` (`BOOL`, Quality Output). `QI` is passed through directly to `Q_NumericValueAux.QI`, and `Q_NumericValueAux.QO` directly to `QO`.

## Summary

AUX counterpart to [`SwitchPic_2_1`](./SwitchPic_2_1.md): same 2-state logic, but for an AUX object instead of a regular VT object.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

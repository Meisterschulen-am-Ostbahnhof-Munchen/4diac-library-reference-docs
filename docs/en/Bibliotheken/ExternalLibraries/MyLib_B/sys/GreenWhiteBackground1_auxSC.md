# GreenWhiteBackground1_auxSC

![GreenWhiteBackground1_auxSC_network](./GreenWhiteBackground1_auxSC_network.svg)

![GreenWhiteBackground1_auxSC](./GreenWhiteBackground1_auxSC.svg)

* * * * * * * * * *

## Introduction

`GreenWhiteBackground1_auxSC` is a **compact wrapper** around [`GreenWhiteBackground1_auxS`](GreenWhiteBackground1_auxS.md) — passes `u16ObjId`s and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

For the shared pattern, see [Background Color Blocks (shared pattern)](../../MyLib_AX/sys/Background-Color-Blocks.md).

In addition, the wrapper exposes the initialization and quality interface: event input `INIT`, event output `INITO`, data input `QI` (`BOOL`, initial value `TRUE`) and data output `QO` (`BOOL`). They are passed 1:1 through to the inner instance `GreenWhiteBackground`: `INIT` → `GreenWhiteBackground.INIT`, `GreenWhiteBackground.INITO` → `INITO`, `QI` → `GreenWhiteBackground.QI`, `GreenWhiteBackground.QO` → `QO`.

## Summary

A pure pass-through to `GreenWhiteBackground1_auxS` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

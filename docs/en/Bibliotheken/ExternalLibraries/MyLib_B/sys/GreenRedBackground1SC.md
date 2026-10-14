# GreenRedBackground1SC

![GreenRedBackground1SC_network](./GreenRedBackground1SC_network.svg)

![GreenRedBackground1SC](./GreenRedBackground1SC.svg)

* * * * * * * * * *

## Introduction

`GreenRedBackground1SC` is a **compact wrapper** around [`GreenRedBackground1S`](GreenRedBackground1S.md) — passes `u16ObjId`s and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

In addition, initialization and quality are passed through: `INIT` → `GreenRedBackground.INIT`, `GreenRedBackground.INITO` → `INITO`, `QI` (initial value `TRUE`) → `GreenRedBackground.QI` and `GreenRedBackground.QO` → `QO`.

For the shared pattern, see [Background Color Blocks (shared pattern)](../../MyLib_AX/sys/Background-Color-Blocks.md).

## Summary

A pure pass-through to `GreenRedBackground1S` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

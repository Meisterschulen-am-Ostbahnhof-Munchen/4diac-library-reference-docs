# GreenWhiteBackground1SC

![GreenWhiteBackground1SC_network](./GreenWhiteBackground1SC_network.svg)

![GreenWhiteBackground1SC](./GreenWhiteBackground1SC.svg)

* * * * * * * * * *

## Introduction

`GreenWhiteBackground1SC` is a **compact wrapper** around [`GreenWhiteBackground1S`](GreenWhiteBackground1S.md) — passes `u16ObjId`s and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

In addition, initialization and quality are passed through: `INIT` → `GreenWhiteBackground.INIT`, `GreenWhiteBackground.INITO` → `INITO`, `QI` (initial value `TRUE`) → `GreenWhiteBackground.QI` and `GreenWhiteBackground.QO` → `QO`.

For the shared pattern, see [Background Color Blocks (shared pattern)](../../MyLib_AX/sys/Background-Color-Blocks.md).

## Summary

A pure pass-through to `GreenWhiteBackground1S` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

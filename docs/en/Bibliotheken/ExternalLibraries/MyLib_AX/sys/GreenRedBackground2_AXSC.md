# GreenRedBackground2_AXSC

![GreenRedBackground2_AXSC_network](./GreenRedBackground2_AXSC_network.svg)

![GreenRedBackground2_AXSC](./GreenRedBackground2_AXSC.svg)

* * * * * * * * * *

## Introduction

`GreenRedBackground2_AXSC` is a **compact wrapper** around [`GreenRedBackground2_AXS`](GreenRedBackground2_AXS.md) — passes `u16ObjId`s and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

In addition, initialization and quality are passed through: `INIT` → `GreenRedBackground.INIT`, `GreenRedBackground.INITO` → `INITO`, `QI` (initial value `TRUE`) → `GreenRedBackground.QI` and `GreenRedBackground.QO` → `QO`.

For the shared pattern, see [Background Color Blocks (shared pattern)](./Background-Color-Blocks.md).

## Summary

A pure pass-through to `GreenRedBackground2_AXS` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

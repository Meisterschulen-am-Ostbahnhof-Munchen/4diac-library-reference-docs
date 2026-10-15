# RedGreenBackground1_AXSC

![RedGreenBackground1_AXSC_network](./RedGreenBackground1_AXSC_network.svg)

![RedGreenBackground1_AXSC](./RedGreenBackground1_AXSC.svg)

* * * * * * * * * *

## Introduction

`RedGreenBackground1_AXSC` is a **compact wrapper** around [`RedGreenBackground1_AXS`](RedGreenBackground1_AXS.md) — passes `u16ObjId`s and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

In addition, initialization and quality are passed through: `INIT` → `RedGreenBackground.INIT`, `RedGreenBackground.INITO` → `INITO`, `QI` (initial value `TRUE`) → `RedGreenBackground.QI` and `RedGreenBackground.QO` → `QO`.

For the shared pattern, see [Background Color Blocks (shared pattern)](./Background-Color-Blocks.md).

## Summary

A pure pass-through to `RedGreenBackground1_AXS` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

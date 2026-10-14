# RedWhiteBackground1_AXSC

![RedWhiteBackground1_AXSC_network](./RedWhiteBackground1_AXSC_network.svg)

![RedWhiteBackground1_AXSC](./RedWhiteBackground1_AXSC.svg)

* * * * * * * * * *

## Introduction

`RedWhiteBackground1_AXSC` is a **compact wrapper** around [`RedWhiteBackground1_AXS`](RedWhiteBackground1_AXS.md) — passes `u16ObjId`s and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

In addition, initialization and quality are passed through: `INIT` → `RedWhiteBackground.INIT`, `RedWhiteBackground.INITO` → `INITO`, `QI` (initial value `TRUE`) → `RedWhiteBackground.QI` and `RedWhiteBackground.QO` → `QO`.

For the shared pattern, see [Background Color Blocks (shared pattern)](./Background-Color-Blocks.md).

## Summary

A pure pass-through to `RedWhiteBackground1_AXS` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

# RedWhiteBackground2_AXSC

![RedWhiteBackground2_AXSC_network](./RedWhiteBackground2_AXSC_network.svg)

![RedWhiteBackground2_AXSC](./RedWhiteBackground2_AXSC.svg)

* * * * * * * * * *

## Introduction

`RedWhiteBackground2_AXSC` is a **compact wrapper** around [`RedWhiteBackground2_AXS`](RedWhiteBackground2_AXS.md) — passes `u16ObjId`s and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`). In addition, `INIT` → `RedWhiteBackground.INIT`, `RedWhiteBackground.INITO` → `INITO`, `QI` → `RedWhiteBackground.QI` (initial value `TRUE`) and `RedWhiteBackground.QO` → `QO` are passed through.

For the shared pattern, see [Background Color Blocks (shared pattern)](./Background-Color-Blocks.md).

## Summary

A pure pass-through to `RedWhiteBackground2_AXS` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

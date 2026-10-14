# GreenBlueBackground1_AXSC

![GreenBlueBackground1_AXSC_network](./GreenBlueBackground1_AXSC_network.svg)

![GreenBlueBackground1_AXSC](./GreenBlueBackground1_AXSC.svg)

* * * * * * * * * *

## Introduction

`GreenBlueBackground1_AXSC` is a **compact wrapper** around [`GreenBlueBackground1_AXS`](GreenBlueBackground1_AXS.md) — passes `u16ObjId`s and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

In addition, initialization and quality are passed through: `INIT` → `GreenBlueBackground.INIT`, `GreenBlueBackground.INITO` → `INITO`, `QI` (initial value `TRUE`) → `GreenBlueBackground.QI` and `GreenBlueBackground.QO` → `QO`.

For the shared pattern, see [Background Color Blocks (shared pattern)](./Background-Color-Blocks.md).

## Summary

A pure pass-through to `GreenBlueBackground1_AXS` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

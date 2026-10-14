# GreenBlueBackground1_aux_AXSC

![GreenBlueBackground1_aux_AXSC_network](./GreenBlueBackground1_aux_AXSC_network.svg)

![GreenBlueBackground1_aux_AXSC](./GreenBlueBackground1_aux_AXSC.svg)

* * * * * * * * * *

## Introduction

`GreenBlueBackground1_aux_AXSC` is a **compact wrapper** around [`GreenBlueBackground1_aux_AXS`](GreenBlueBackground1_aux_AXS.md) — passes `u16ObjId`s and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

For the shared pattern, see [Background Color Blocks (shared pattern)](./Background-Color-Blocks.md).

In addition, the wrapper exposes the initialization and quality interface: event input `INIT`, event output `INITO`, data input `QI` (`BOOL`, initial value `TRUE`) and data output `QO` (`BOOL`). They are passed 1:1 through to the inner instance `GreenBlueBackground`: `INIT` → `GreenBlueBackground.INIT`, `GreenBlueBackground.INITO` → `INITO`, `QI` → `GreenBlueBackground.QI`, `GreenBlueBackground.QO` → `QO`.

## Summary

A pure pass-through to `GreenBlueBackground1_aux_AXS` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

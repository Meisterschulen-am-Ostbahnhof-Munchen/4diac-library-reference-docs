# GreenRedBackground1_AXSC

![GreenRedBackground1_AXSC_network](./GreenRedBackground1_AXSC_network.svg)

![GreenRedBackground1_AXSC](./GreenRedBackground1_AXSC.svg)

* * * * * * * * * *

## Introduction

`GreenRedBackground1_AXSC` is a **compact wrapper** around [`GreenRedBackground1_AXS`](GreenRedBackground1_AXS.md) — passes `u16ObjId`s and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

For the shared pattern, see [Background Color Blocks (shared pattern)](./Background-Color-Blocks.md).

## Initialization and Quality

- **Event input `INIT`** (`EInit`) and **event output `INITO`** (`EInit`) are passed through: `INIT` → `GreenRedBackground.INIT`; `GreenRedBackground.INITO` → `INITO`.
- **Data input `QI`** (`BOOL`, default `TRUE`) → `GreenRedBackground.QI`; **data output `QO`** (`BOOL`) ← `GreenRedBackground.QO`.

The inner SubApp instance `GreenRedBackground` is of type `GreenRedBackground1_AXS`.

## Summary

A pure pass-through to `GreenRedBackground1_AXS` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

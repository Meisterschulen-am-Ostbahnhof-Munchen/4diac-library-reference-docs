# RedWhiteBackground2SC

![RedWhiteBackground2SC_network](./RedWhiteBackground2SC_network.svg)

![RedWhiteBackground2SC](./RedWhiteBackground2SC.svg)

* * * * * * * * * *

## Introduction

`RedWhiteBackground2SC` is a **compact wrapper** around [`RedWhiteBackground2S`](RedWhiteBackground2S.md) — passes `u16ObjId`s and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

Interface (quality pattern): The wrapper additionally has the event input `INIT` and the event output `INITO` (each `EInit`) as well as the data input `QI` (`BOOL`, default `TRUE`) and the data output `QO` (`BOOL`). They are passed straight through to the inner instance `RedWhiteBackground`: `INIT` → `RedWhiteBackground.INIT`, `RedWhiteBackground.INITO` → `INITO`, `QI` → `RedWhiteBackground.QI`, `RedWhiteBackground.QO` → `QO`.

For the shared pattern, see [Background Color Blocks (shared pattern)](../../MyLib_AX/sys/Background-Color-Blocks.md).

## Summary

A pure pass-through to `RedWhiteBackground2S` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

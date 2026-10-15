# RedGreenBackground4_AXC

![RedGreenBackground4_AXC_network](./RedGreenBackground4_AXC_network.svg)

![RedGreenBackground4_AXC](./RedGreenBackground4_AXC.svg)

* * * * * * * * * *

## Introduction

`RedGreenBackground4_AXC` is a **compact wrapper** around [`RedGreenBackground4_AX`](RedGreenBackground4_AX.md) — passes `u16ObjId` and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

Interface (quality pattern): The wrapper additionally has the event input `INIT` and the event output `INITO` (each `EInit`) as well as the data input `QI` (`BOOL`, default `TRUE`) and the data output `QO` (`BOOL`). They are passed straight through to the inner instance `RedGreenBackground4`: `INIT` → `RedGreenBackground4.INIT`, `RedGreenBackground4.INITO` → `INITO`, `QI` → `RedGreenBackground4.QI`, `RedGreenBackground4.QO` → `QO`.

For the shared pattern, see [Background Color Blocks (shared pattern)](./Background-Color-Blocks.md).

## Summary

A pure pass-through to `RedGreenBackground4_AX` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

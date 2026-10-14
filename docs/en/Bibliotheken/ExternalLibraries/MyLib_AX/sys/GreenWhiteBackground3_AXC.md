# GreenWhiteBackground3_AXC

![GreenWhiteBackground3_AXC_network](./GreenWhiteBackground3_AXC_network.svg)

![GreenWhiteBackground3_AXC](./GreenWhiteBackground3_AXC.svg)

* * * * * * * * * *

## Introduction

`GreenWhiteBackground3_AXC` is a **compact wrapper** around [`GreenWhiteBackground3_AX`](GreenWhiteBackground3_AX.md) — passes `u16ObjId` and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

Interface (quality pattern): The wrapper additionally has the event input `INIT` and the event output `INITO` (each `EInit`) as well as the data input `QI` (`BOOL`, default `TRUE`) and the data output `QO` (`BOOL`). They are passed straight through to the inner instance `GreenWhiteBackground3`: `INIT` → `GreenWhiteBackground3.INIT`, `GreenWhiteBackground3.INITO` → `INITO`, `QI` → `GreenWhiteBackground3.QI`, `GreenWhiteBackground3.QO` → `QO`.

For the shared pattern, see [Background Color Blocks (shared pattern)](./Background-Color-Blocks.md).

## Summary

A pure pass-through to `GreenWhiteBackground3_AX` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

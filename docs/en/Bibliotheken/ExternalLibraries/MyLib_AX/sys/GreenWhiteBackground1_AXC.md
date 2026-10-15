# GreenWhiteBackground1_AXC

![GreenWhiteBackground1_AXC_network](./GreenWhiteBackground1_AXC_network.svg)

![GreenWhiteBackground1_AXC](./GreenWhiteBackground1_AXC.svg)

* * * * * * * * * *

## Introduction

`GreenWhiteBackground1_AXC` is a **compact wrapper** around [`GreenWhiteBackground1_AX`](GreenWhiteBackground1_AX.md) — passes `u16ObjId` and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

For the shared pattern, see [Background Color Blocks (shared pattern)](./Background-Color-Blocks.md).

In addition, the wrapper exposes the initialization and quality interface: event input `INIT`, event output `INITO`, data input `QI` (`BOOL`, initial value `TRUE`) and data output `QO` (`BOOL`). They are passed 1:1 through to the inner instance `GreenWhiteBackground1`: `INIT` → `GreenWhiteBackground1.INIT`, `GreenWhiteBackground1.INITO` → `INITO`, `QI` → `GreenWhiteBackground1.QI`, `GreenWhiteBackground1.QO` → `QO`.

## Summary

A pure pass-through to `GreenWhiteBackground1_AX` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

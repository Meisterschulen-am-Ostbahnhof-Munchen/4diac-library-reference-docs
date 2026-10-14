# GreenRedBackground4SC

![GreenRedBackground4SC_network](./GreenRedBackground4SC_network.svg)

![GreenRedBackground4SC](./GreenRedBackground4SC.svg)

* * * * * * * * * *

## Introduction

`GreenRedBackground4SC` is a **compact wrapper** around [`GreenRedBackground4S`](GreenRedBackground4S.md) — passes `u16ObjId`s and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

For the shared pattern, see [Background Color Blocks (shared pattern)](../../MyLib_AX/sys/Background-Color-Blocks.md).

## Initialization and Status (INIT/INITO, QI/QO)

- **Event input** `INIT` (`EInit`, Initialization Request) and **event output** `INITO` (`EInit`, Initialization Confirm)
- **Data input** `QI` (`BOOL`, default `TRUE`, Quality Input) is forwarded to `GreenRedBackground4.QI`
- **Data output** `QO` (`BOOL`, Quality Output): passed through from `GreenRedBackground4.QO`
- **Initialization chain**: `INIT` → `GreenRedBackground4.INIT`, `GreenRedBackground4.INITO` → `INITO`

## Summary

A pure pass-through to `GreenRedBackground4S` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

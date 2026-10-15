# RedGreenBackground1SC

![RedGreenBackground1SC_network](./RedGreenBackground1SC_network.svg)

![RedGreenBackground1SC](./RedGreenBackground1SC.svg)

* * * * * * * * * *

## Introduction

`RedGreenBackground1SC` is a **compact wrapper** around [`RedGreenBackground1S`](RedGreenBackground1S.md) — passes `u16ObjId`s and the selector straight through, without exposing the `CNF` event outputs and diagnostic values (`STATUS_n`, `u8OldColour_n`, `result_n`).

For the shared pattern, see [Background Color Blocks (shared pattern)](../../MyLib_AX/sys/Background-Color-Blocks.md).

## Initialization and Status (INIT/INITO, QI/QO)

- **Event input** `INIT` (`EInit`, Initialization Request) and **event output** `INITO` (`EInit`, Initialization Confirm)
- **Data input** `QI` (`BOOL`, default `TRUE`, Quality Input) is forwarded to `RedGreenBackground.QI`
- **Data output** `QO` (`BOOL`, Quality Output): passed through from `RedGreenBackground.QO`
- **Initialization chain**: `INIT` → `RedGreenBackground.INIT`, `RedGreenBackground.INITO` → `INITO`

## Summary

A pure pass-through to `RedGreenBackground1S` for the common case where the diagnostic values aren't needed.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

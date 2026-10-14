# NVS_IN_AND_STORE_AR

![NVS_IN_AND_STORE_AR_network](./NVS_IN_AND_STORE_AR_network.svg)

* * * * * * * * * *

## Introduction

`NVS_IN_AND_STORE_AR` persistently stores a physically scaled REAL value entered via the VT (AR adapter) in ESP32 flash (NVS) — otherwise identical to [`INI_IN_AND_STORE_AR`](./INI_IN_AND_STORE_AR.md).

For the general pattern, see [INI_IN_AND_STORE / NVS_IN_AND_STORE (shared pattern)](./INI-NVS-Storage-Blocks.md).

![NVS_IN_AND_STORE_AR](NVS_IN_AND_STORE_AR.svg)

## Quality ports (QI/QO)

The block has the data input `QI` (`BOOL`, default `TRUE`, Quality Input - TRUE enables the service) and the data output `QO` (`BOOL`, Quality Output - TRUE if the service was executed successfully). `QI` is connected to `Q_NumericValue_PHYSA.QI`, and `Q_NumericValue_PHYSA.QO` to `QO`.

In addition, the block has the events `INIT` (input, `EInit`, initialization request) and `INITO` (output, `EInit`, initialization confirmation): `INIT` → `Q_NumericValue_PHYSA.INIT`, `Q_NumericValue_PHYSA.INITO` → `INITO`.

## Summary

NVS counterpart to `INI_IN_AND_STORE_AR`.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

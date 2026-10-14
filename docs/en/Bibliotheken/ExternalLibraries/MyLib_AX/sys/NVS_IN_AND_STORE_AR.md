# NVS_IN_AND_STORE_AR

![NVS_IN_AND_STORE_AR_network](./NVS_IN_AND_STORE_AR_network.svg)

* * * * * * * * * *

## Introduction

`NVS_IN_AND_STORE_AR` persistently stores a physically scaled REAL value entered via the VT (AR adapter) in ESP32 flash (NVS) — otherwise identical to [`INI_IN_AND_STORE_AR`](./INI_IN_AND_STORE_AR.md).

For the general pattern, see [INI_IN_AND_STORE / NVS_IN_AND_STORE (shared pattern)](./INI-NVS-Storage-Blocks.md).

![NVS_IN_AND_STORE_AR](NVS_IN_AND_STORE_AR.svg)

## Quality ports (QI/QO)

The block has the data input `QI` (`BOOL`, default `TRUE`, Quality Input - TRUE enables the service) and the data output `QO` (`BOOL`, Quality Output - TRUE if the service was executed successfully). `QI` is connected to `Q_NumericValue_PHYSA.QI`, `NVS_AR.QI` and `NumericValue_PHYSA.QI`; `QO` is the AND (`AND_QO`, `iec61131::booleanOperators::AND_BOOL_3`) of `Q_NumericValue_PHYSA.QO`, `NVS_AR.QO` and `NumericValue_PHYSA.QO`.

In addition, the block has the events `INIT` (input, `EInit`, initialization request) and `INITO` (output, `EInit`, initialization confirmation): initialization is chained: `INIT` → `Q_NumericValue_PHYSA.INIT`; `Q_NumericValue_PHYSA.INITO` → `NVS_AR.INIT`; `NVS_AR.INITO` → `NumericValue_PHYSA.INIT`; `NumericValue_PHYSA.INITO` → `INITO` (and → `AND_QO.REQ`).

## Summary

NVS counterpart to `INI_IN_AND_STORE_AR`.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

# INI_IN_AND_STORE_AR

![INI_IN_AND_STORE_AR_network](./INI_IN_AND_STORE_AR_network.svg)

* * * * * * * * * *

## Introduction

`INI_IN_AND_STORE_AR` persistently stores a physically scaled REAL value entered via the VT (AR adapter, `NumericValue_PHYSA`/`Q_NumericValue_PHYSA` with `NumericObjectPool_S` scaling: `r32Scale`, `i32Offset`, `u8Decimals`) in an INI file.

For the general pattern, see [INI_IN_AND_STORE / NVS_IN_AND_STORE (shared pattern)](./INI-NVS-Storage-Blocks.md).

Interface (quality pattern): event input `INIT` and event output `INITO` (each `EInit`), data input `QI` (`BOOL`, default `TRUE`, "Quality Input - TRUE to enable service") and data output `QO` (`BOOL`, "Quality Output - TRUE if service executed successfully"). Initialization: `INIT` → `NumericValue_PHYSA.INIT`; `NumericValue_PHYSA.INITO` → `INI_AR.INIT`; `INI_AR.INITO` → `Q_NumericValue_PHYSA.INIT`; `Q_NumericValue_PHYSA.INITO` → `INITO` and → `AND_QO.REQ`. Quality: `QI` → `Q_NumericValue_PHYSA.QI`, `NumericValue_PHYSA.QI`, `INI_AR.QI`; `Q_NumericValue_PHYSA.QO`, `NumericValue_PHYSA.QO`, `INI_AR.QO` → `AND_QO.IN1` to `AND_QO.IN3` (in this order); `AND_QO.OUT` → `QO` (`AND_QO`: `iec61131::booleanOperators::AND_BOOL_3`).

![INI_IN_AND_STORE_AR](INI_IN_AND_STORE_AR.svg)

## Summary

AR (physically scaled REAL) variant of the INI storage family.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

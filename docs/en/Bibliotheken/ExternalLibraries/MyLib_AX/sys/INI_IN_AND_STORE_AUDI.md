# INI_IN_AND_STORE_AUDI

![INI_IN_AND_STORE_AUDI_network](./INI_IN_AND_STORE_AUDI_network.svg)

* * * * * * * * * *

## Introduction

`INI_IN_AND_STORE_AUDI` persistently stores a UDINT value entered via the VT (AUDI adapter) in an INI file.

For the general pattern, see [INI_IN_AND_STORE / NVS_IN_AND_STORE (shared pattern)](./INI-NVS-Storage-Blocks.md).

![INI_IN_AND_STORE_AUDI](INI_IN_AND_STORE_AUDI.svg)

## Event and quality ports (INIT/INITO, QI/QO)

The block has the event input `INIT` and the event output `INITO` (both `EInit`), the data input `QI` (`BOOL`, default `TRUE`, Quality Input - TRUE enables the service) and the data output `QO` (`BOOL`, Quality Output - TRUE if the service was executed successfully).

Initialization runs as a chain through the inner blocks: `INIT` → `ID.INIT`, `ID.INITO` → `INI_AUDI.INIT`, `INI_AUDI.INITO` → `Q_NumericValue_AUDI.INIT`, `Q_NumericValue_AUDI.INITO` → `INITO` (and → `AND_QO.REQ`). `QI` is passed to `ID`, `INI_AUDI` and `Q_NumericValue_AUDI`. `QO` is the output of the instance `AND_QO` (`iec61131::booleanOperators::AND_BOOL_3`), which combines `Q_NumericValue_AUDI.QO`, `INI_AUDI.QO` and `ID.QO`.

## Summary

AUDI (UDINT) variant of the INI storage family.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

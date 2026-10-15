# INI_IN_AND_STORE_AIS

![INI_IN_AND_STORE_AIS_network](./INI_IN_AND_STORE_AIS_network.svg)

* * * * * * * * * *

## Introduction

`INI_IN_AND_STORE_AIS` persistently stores a string value entered via the VT (AIS adapter) in an INI file and automatically reloads it on deployment.

For the general pattern, see [INI_IN_AND_STORE / NVS_IN_AND_STORE (shared pattern)](./INI-NVS-Storage-Blocks.md).

Interface (quality pattern): the data input `QI` (`BOOL`, default `TRUE`, "Quality Input - TRUE to enable service") and the data output `QO` (`BOOL`, "Quality Output - TRUE if service executed successfully") are connected to all three internal service blocks (`StringValue_AIS`, `INI_AIS`, `Q_StringValue_AIS`): `QI` → `Q_StringValue_AIS.QI`, `INI_AIS.QI` and `StringValue_AIS.QI`; `QO` is the AND (`AND_QO`, `iec61131::booleanOperators::AND_BOOL_3`) of `Q_StringValue_AIS.QO`, `INI_AIS.QO` and `StringValue_AIS.QO`.

In addition, the block has the events `INIT` (input, `EInit`) and `INITO` (output, `EInit`); initialization is chained: `INIT` → `StringValue_AIS.INIT`; `StringValue_AIS.INITO` → `INI_AIS.INIT`; `INI_AIS.INITO` → `Q_StringValue_AIS.INIT`; `Q_StringValue_AIS.INITO` → `INITO` (and → `AND_QO.REQ`).

![INI_IN_AND_STORE_AIS](INI_IN_AND_STORE_AIS.svg)

## Summary

AIS (string) variant of the INI storage family.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

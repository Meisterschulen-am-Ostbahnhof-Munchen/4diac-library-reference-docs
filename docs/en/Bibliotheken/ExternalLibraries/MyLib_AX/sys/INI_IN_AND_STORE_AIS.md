# INI_IN_AND_STORE_AIS

![INI_IN_AND_STORE_AIS_network](./INI_IN_AND_STORE_AIS_network.svg)

* * * * * * * * * *

## Introduction

`INI_IN_AND_STORE_AIS` persistently stores a string value entered via the VT (AIS adapter) in an INI file and automatically reloads it on deployment.

For the general pattern, see [INI_IN_AND_STORE / NVS_IN_AND_STORE (shared pattern)](./INI-NVS-Storage-Blocks.md).

Interface (quality pattern): the data input `QI` (`BOOL`, default `TRUE`, "Quality Input - TRUE to enable service") and the data output `QO` (`BOOL`, "Quality Output - TRUE if service executed successfully") are connected directly to the VT write block: `QI` → `Q_StringValue_AIS.QI`, `Q_StringValue_AIS.QO` → `QO`.

![INI_IN_AND_STORE_AIS](INI_IN_AND_STORE_AIS.svg)

## Summary

AIS (string) variant of the INI storage family.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

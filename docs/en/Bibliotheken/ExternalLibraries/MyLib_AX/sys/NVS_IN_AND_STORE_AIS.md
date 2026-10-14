# NVS_IN_AND_STORE_AIS

![NVS_IN_AND_STORE_AIS_network](./NVS_IN_AND_STORE_AIS_network.svg)

* * * * * * * * * *

## Introduction

`NVS_IN_AND_STORE_AIS` persistently stores a string value entered via the VT (AIS adapter) in ESP32 flash (NVS) instead of an INI file — otherwise identical to [`INI_IN_AND_STORE_AIS`](./INI_IN_AND_STORE_AIS.md) (no `SECTION` parameter, since NVS is organized differently from INI files).

For the general pattern, see [INI_IN_AND_STORE / NVS_IN_AND_STORE (shared pattern)](./INI-NVS-Storage-Blocks.md).

![NVS_IN_AND_STORE_AIS](NVS_IN_AND_STORE_AIS.svg)

## Quality ports (QI/QO)

The block has the data input `QI` (`BOOL`, default `TRUE`, Quality Input - TRUE enables the service) and the data output `QO` (`BOOL`, Quality Output - TRUE if the service was executed successfully). `QI` is connected to `Q_StringValue_AIS.QI`, and `Q_StringValue_AIS.QO` to `QO`.

In addition, the block has the events `INIT` (input, `EInit`, initialization request) and `INITO` (output, `EInit`, initialization confirmation): `INIT` → `Q_StringValue_AIS.INIT`, `Q_StringValue_AIS.INITO` → `INITO`.

## Summary

NVS counterpart to `INI_IN_AND_STORE_AIS`.

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

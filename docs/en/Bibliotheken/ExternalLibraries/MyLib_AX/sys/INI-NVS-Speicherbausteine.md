# INI_IN_AND_STORE / NVS_IN_AND_STORE: Common Pattern

* * * * * * * * * *

## Introduction

`MyLib::sys` contains a family of function blocks that **persistently store** a value entered via VT and automatically reload it during deployment—either in an INI file (`INI_IN_AND_STORE_*`) or in ESP32 flash memory (NVS, `NVS_IN_AND_STORE_*`). This page explains the common pattern.



## Naming Scheme

`{INI|NVS}_IN_AND_STORE_<Typ>`, where `<Typ>` determines the data type: `AIS` (string adapter), `AR` (REAL adapter, with physical scaling `NumericObjectPool_S`), `AUDI` (UDINT adapter) in test_AX; `UDINT` (classic, without adapter) in test_B.

## Functionality (test_AX, adapter-based)

1. A VT input field (`StringValue_AIS`/`NumericValue_PHYSA`/…) returns the value entered by the user as an adapter.

2. `INI_<Typ>`/`NVS_<Typ>` (`eclipse4diac::storage::INI_*` or `logiBUS::storage::esp32_nvs::NVS_*`) persistently stores this value under `KEY`/`SECTION` (INI only) and returns the last saved value on `INIT` (`SETM=TRUE`: saving active, `DEFAULT_VALUE`: initial value if nothing has been saved yet).

3. The (newly loaded or newly entered) value is distributed via `<Typ>_SPLIT_2`: once as a plug `VALUEO` to the outside (for further use in the calling network) and once to `Q_StringValue_AIS`/`Q_NumericValue_PHYSA` (written back to the VT display field so that the input field and display remain synchronized).


## Functionality (test_B, classic)

The `test_B` variant (`INI_IN_AND_STORE_UDINT`/`NVS_IN_AND_STORE_UDINT`) is older and uses classic event/data connections instead of adapters: A `NumericValue_ID` input field returns a DWORD value, which is converted to UDINT via `F_DWORD_TO_UDINT` and passed to `INI`/`NVS` (generic, non-type-specific blocks). The stored value is sent out as `VALUEO` and back to `Q_NumericValue` for display.



## Summary

Both approaches solve the same problem—"remembering a user-entered value across restarts"—using the same basic pattern (input field → memory function block with KEY/SECTION → branching to output + display writeback), only with different storage locations (INI file vs. NVS flash) and different wiring styles (adapter in test_AX, classic in test_B).

---

### 🌐 Related topic subpages on ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

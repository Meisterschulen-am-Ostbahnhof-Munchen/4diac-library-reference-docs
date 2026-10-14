# INI_IN_AND_STORE_AUI2

![INI_IN_AND_STORE_AUI2_network](./INI_IN_AND_STORE_AUI2_network.svg)

* * * * * * * * * *

## Introduction

`INI_IN_AND_STORE_AUI2` loads and stores AUI2 data (pure integer 0-N, no scale/decimals) in an INI file and additionally provides `VALUEO` (AUI, plain) for a further consumer. It corresponds to [`INI_IN_AND_STORE_AUDI2`](./INI_IN_AND_STORE_AUDI2.md), but uses the bidirectional AUI2 adapter (UINT) instead of AUDI2 (UDINT) - e.g. for `K` of an `AOption_AUI_DEMUX_N`.

For the general pattern, see [INI_IN_AND_STORE / NVS_IN_AND_STORE (shared pattern)](./INI-NVS-Storage-Blocks.md).

![INI_IN_AND_STORE_AUI2](INI_IN_AND_STORE_AUI2.svg)

## Interface

### Input variables

- `QI` (BOOL, default `TRUE`): Quality Input - TRUE enables the service.
- `KEY` (STRING): key name in the INI file.
- `SECTION` (STRING): section name in the INI file.
- `u16ObjId` (UINT, default `ID_NULL`): ObjectID of the VT input field.

### Output variables

- `QO` (BOOL): Quality Output - TRUE if the service was executed successfully (AND of `Q_NumericValue_AUDI.QO`, `ID.QO` and `INI_AUI2.QO`).

### Adapters

- **Plug `VALUEO`** (`adapter::types::unidirectional::AUI`): value for a further consumer.

### Events

- Input `INIT` (`EInit`): Initialization Request
- Output `INITO` (`EInit`): Initialization Confirm

## How it works

1. `ID` (`NumericValue_IDA`) reads the VT input field; `AD_TO_AUI` converts to AUI.
2. `AUI_TO_AUI2` converts into the bidirectional AUI2 adapter for `INI_AUI2` (`eclipse4diac::storage::INI_AUI2`; `SETM`=TRUE, `DEFAULT_VALUE`=`UINT#0`).
3. `AUI_TO_AUI2.AUI_OUT` is duplicated via `AUI_SPLIT_2`: `OUT1` to `VALUEO`, `OUT2` via `AUI_TO_AUDI` to `Q_NumericValue_AUDI` (`u32NewValue`, VT display).

`QI` is passed through to `ID.QI`, `INI_AUI2.QI` and `Q_NumericValue_AUDI.QI`. The initialization chain is `INIT` → `ID.INIT`, `ID.INITO` → `INI_AUI2.INIT`, `INI_AUI2.INITO` → `Q_NumericValue_AUDI.INIT`, `Q_NumericValue_AUDI.INITO` → `INITO` (and additionally → `AND_QO.REQ`). `QO` is the AND (`AND_QO`, type `iec61131::booleanOperators::AND_BOOL_3`) of `Q_NumericValue_AUDI.QO`, `ID.QO` and `INI_AUI2.QO`.

## Technical notes

- Structurally identical to `INI_IN_AND_STORE_AUDI2`, only with AUI2 instead of AUDI2.
- `SETM`=TRUE: the boot value loads automatically (INIT → INITO → GET → GETO); `SETM`=TRUE additionally ensures that a live change is confirmed/echoed immediately (e.g. for further use via `VALUEO`).

## Summary

AUI2 variant of the INI storage family for pure integers with an additional `VALUEO` output.

## 🛠️ Related Exercises

- [Uebung_094b4_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b4_AX/)

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

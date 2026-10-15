# NumericValue_TO_AUI2_AND_STORE

![NumericValue_TO_AUI2_AND_STORE_network](./NumericValue_TO_AUI2_AND_STORE_network.svg)

* * * * * * * * * *

## Introduction

`NumericValue_TO_AUI2_AND_STORE` corresponds to [`NumericValue_TO_AUI2`](./NumericValue_TO_AUI2.md) (read the VT, pass it as a bidirectional AUI2 plug `VALUEO` to a consumer such as `AOption_AUI_DEMUX_N.K`), extended with INI persistence: at boot the value loaded from the INI file wins (merged against the live VT reading via `AUI_LAST_2`), after that every live change. What is stored is always the value **reported back** by the consumer (the one actually accepted). The block is its own class, separate from `INI_IN_AND_STORE_AUI2`/`_AUDI2`, which remain unchanged (KEY/SECTION, plain `VALUEO`) because they are used in numerous other projects.

For the general pattern, see [INI_IN_AND_STORE / NVS_IN_AND_STORE (shared pattern)](./INI-NVS-Storage-Blocks.md).

![NumericValue_TO_AUI2_AND_STORE](NumericValue_TO_AUI2_AND_STORE.svg)

## Interface

### Event inputs

- `INIT` (`EInit`): initialization request.

### Event outputs

- `INITO` (`EInit`): initialization confirm.

### Input variables

- `QI` (BOOL, default `TRUE`): Quality Input - TRUE enables the service.
- `KEY` (STRING): key name in the INI file.
- `SECTION` (STRING): section name in the INI file.
- `u16ObjId` (UINT, default `ID_NULL`): ObjectID of the VT input field.

### Output variables

- `QO` (BOOL): Quality Output - TRUE if the service was executed successfully (AND combination `AND_QO` of the `QO` outputs of the inner blocks).

### Adapters

- **Plug `VALUEO`** (`adapter::types::bidirectional::AUI2`): value to the consumer and echo back.

The events `INIT`/`INITO` are passed as a chain through the inner blocks (see How it works).

## How it works

1. `NumericValue_IDA` reads the VT input field; `AD_TO_AUI` converts to AUI and feeds `AUI_LAST_2.IN1`.
2. `AUI_LAST_2` (last writer wins) merges: `IN1` = live VT change, `IN2` = INI value at boot. Its output goes to `AUI_TO_AUI2` (bridge to the external consumer), whose bidirectional output goes to `VALUEO`.
3. The result reported back by the consumer appears at `AUI_TO_AUI2.AUI_OUT` and is duplicated via `AUI_SPLIT_2`: `OUT1` via `AUI_TO_AUDI` to `Q_NumericValue_AUDI` (`u32NewValue`, VT display under `u16ObjId`), `OUT2` to `INI_AUI` (`eclipse4diac::storage::INI_AUI`, `AUI_IN`; `SETM`=FALSE, `DEFAULT_VALUE`=`UINT#0`).
4. `INI_AUI.AUI_OUT` feeds the loaded value back to `AUI_LAST_2.IN2`.

Initialization: `INIT` → `NumericValue_IDA.INIT`; `NumericValue_IDA.INITO` → `INI_AUI.INIT`; `INI_AUI.INITO` → `Q_NumericValue_AUDI.INIT`; `Q_NumericValue_AUDI.INITO` → `INITO` and → `AND_QO.REQ`. Quality: `QI` → `Q_NumericValue_AUDI.QI`, `NumericValue_IDA.QI`, `INI_AUI.QI`; `Q_NumericValue_AUDI.QO`, `NumericValue_IDA.QO`, `INI_AUI.QO` → `AND_QO.IN1` to `AND_QO.IN3` (in this order); `AND_QO.OUT` → `QO` (`AND_QO`: `iec61131::booleanOperators::AND_BOOL_3`).

## Technical notes

- `SETM`=FALSE: `INI_AUI` acknowledges automatically only at boot (INIT → INITO → GET → GETO), not on every later write. With `SETM`=TRUE every write would trigger a new echo that would arrive at `AUI_LAST_2` again and be sent towards `VALUEO` - an endless loop.
- The live VT reading goes directly to `VALUEO` (a single `AUI_TO_AUI2`); only the result reported back by the consumer is stored, via the split.
- The boot value wins once in `AUI_LAST_2` against the live VT reading.

## Summary

Like `NumericValue_TO_AUI2`, with INI persistence of the consumer-confirmed value and a boot value merge.

## 🛠️ Related Exercises

- [Uebung_094b3b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b3b_AX/)
- [Uebung_094b4b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b4b_AX/)
- [Uebung_094c2b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094c2b_AX/)

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

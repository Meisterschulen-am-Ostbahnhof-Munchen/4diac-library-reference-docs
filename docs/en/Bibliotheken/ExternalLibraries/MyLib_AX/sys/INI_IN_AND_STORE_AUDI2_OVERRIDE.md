# INI_IN_AND_STORE_AUDI2_OVERRIDE

![INI_IN_AND_STORE_AUDI2_OVERRIDE_network](./INI_IN_AND_STORE_AUDI2_OVERRIDE_network.svg)

* * * * * * * * * *

## Introduction

`INI_IN_AND_STORE_AUDI2_OVERRIDE` is like [`INI_IN_AND_STORE_AUDI2`](./INI_IN_AND_STORE_AUDI2.md), but with an override option: while the `OVERRIDE_ACTIVE` socket (AX) is continuously TRUE, the value 1 (disabled) is forced instead of the live VT value - e.g. for mutual option exclusion. With `OVERRIDE_ACTIVE`=FALSE (unused) the behavior is identical to `INI_IN_AND_STORE_AUDI2`. The variant without `OVERRIDE_ACTIVE` is available separately as `INI_IN_AND_STORE_AUDI2` (rename in version 1.2).

For the general pattern, see [INI_IN_AND_STORE / NVS_IN_AND_STORE (shared pattern)](./INI-NVS-Storage-Blocks.md).

![INI_IN_AND_STORE_AUDI2_OVERRIDE](INI_IN_AND_STORE_AUDI2_OVERRIDE.svg)

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

- **Plug `VALUEO`** (`adapter::types::unidirectional::AUDI`): value for a further consumer.
- **Socket `OVERRIDE_ACTIVE`** (`adapter::types::unidirectional::AX`): when continuously TRUE, VAL=1 (disabled) is forced.

The events `INIT`/`INITO` are passed as a chain through the inner blocks (see How it works).

## How it works

1. `ID` (`NumericValue_IDA`) reads the VT input field; `AD_TO_AUDI` converts to AUDI.
2. `SEL_Override` (`AUDI_AX_SEL_AUDI`) selects: `IN0` = live VT value, `IN1` = override value from `LIT_Deaktiviert` (`initval_AUDI`, `INIT_VAL`=`UDINT#1`), selected via `G` = `OVERRIDE_ACTIVE`.
3. `AUDI_TO_AUDI2` converts into the bidirectional AUDI2 adapter for `INI_AUDI2` (`eclipse4diac::storage::INI_AUDI2`; `SETM`=TRUE, `DEFAULT_VALUE`=`UDINT#0`).
4. `AUDI_TO_AUDI2.AUDI_OUT` is duplicated via `AUDI_SPLIT_2`: `OUT1` to `VALUEO`, `OUT2` to `Q_NumericValue_AUDI` (`u32NewValue`, VT display).

Initialization: `INIT` → `ID.INIT`; `ID.INITO` → `INI_AUDI2.INIT`; `INI_AUDI2.INITO` → `Q_NumericValue_AUDI.INIT`; `Q_NumericValue_AUDI.INITO` → `INITO` and → `AND_QO.REQ`. Quality: `QI` → `Q_NumericValue_AUDI.QI`, `ID.QI`, `INI_AUDI2.QI`; `Q_NumericValue_AUDI.QO`, `ID.QO`, `INI_AUDI2.QO` → `AND_QO.IN1` to `AND_QO.IN3` (in this order); `AND_QO.OUT` → `QO` (`AND_QO`: `iec61131::booleanOperators::AND_BOOL_3`).

## Technical notes

- The override runs over the same `INI_AUDI2`/`Q_NumericValue_AUDI` path: according to its own documentation, `Q_NumericValue` allows only one instance per ObjectID, so a second writing instance is not possible.
- For pure enum/integer values (e.g. dropdowns 0-3), no decimal places.
- `SETM`=TRUE: the boot value loads automatically (INIT → INITO → GET → GETO); `SETM`=TRUE additionally ensures that a live change is confirmed/echoed immediately.

## Summary

AUDI2 storage block with a switchable override to 1 (disabled), e.g. for mutual option exclusion.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

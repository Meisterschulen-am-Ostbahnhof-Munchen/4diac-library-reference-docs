# NumericValue_TO_AD2

![NumericValue_TO_AD2_network](./NumericValue_TO_AD2_network.svg)

* * * * * * * * * *

## Introduction

`NumericValue_TO_AD2` reads a VT input field (CInputNumber) and passes the value bidirectionally as an AD2 adapter (DWORD/bitmask, `VALUEO`) to a consumer (e.g. `AOption_AD_DEMUX_N.K`). The block does **not** store anything itself: the consumer reports the bitmask it actually accepted back over the same AD2 plug, and the echo is written back to the VT field.

![NumericValue_TO_AD2](NumericValue_TO_AD2.svg)

## Interface

### Input variables

- `QI` (BOOL, default `TRUE`): Quality Input - TRUE enables the service; passed through to `NumericValue_IDA` and `Q_NumericValue_AUDI`.
- `u16ObjId` (UINT, default `ID_NULL`): ObjectID of the VT input field.

### Output variables

- `QO` (BOOL): Quality Output - TRUE if the service was executed successfully; `AND` of the `QO` outputs of `Q_NumericValue_AUDI` and `NumericValue_IDA` (via `AND_QO`).

### Adapters

- **Plug `VALUEO`** (`adapter::types::bidirectional::AD2`): bitmask to the consumer and echo back.

### Events

- `INIT` (EInit, input): initialization request; starts the chain `NumericValue_IDA` → `Q_NumericValue_AUDI`.
- `INITO` (EInit, output): initialization confirmation; comes from `Q_NumericValue_AUDI.INITO`.

## How it works

1. `NumericValue_IDA` reads the VT input field.
2. `AD_TO_AD2` converts the value into the bidirectional AD2 adapter, which is output on `VALUEO`.
3. The echo reported back by the consumer appears at `AD_TO_AD2.AD_OUT`, is converted to UDINT via `AD_TO_AUDI`, and written to the VT field under `u16ObjId` by `Q_NumericValue_AUDI` (`u32NewValue`).

`QI` is passed through to `NumericValue_IDA.QI` and `Q_NumericValue_AUDI.QI` (no fixed `QI`=TRUE any more). Initialization runs as the chain `INIT` → `NumericValue_IDA.INIT`, `NumericValue_IDA.INITO` → `Q_NumericValue_AUDI.INIT`, `Q_NumericValue_AUDI.INITO` → `INITO` and → `AND_QO.REQ`. `QO` is the `AND` (`AND_QO`, `AND_BOOL_2`) of the `QO` outputs of `Q_NumericValue_AUDI` and `NumericValue_IDA`.

## Technical notes

- DWORD/bitmask counterpart of [`NumericValue_TO_AUI2`](./NumericValue_TO_AUI2.md) - for `AOption_AD_DEMUX_N` (multi-select) instead of `AOption_AUI_DEMUX_N` (single-select).
- No `AD_TO_AUI` intermediate step is needed, since AD2 is already a DWORD.
- Pure VT-to-AD2 bridge without a storage FB; compare [`NumericValue_TO_AR2`](./NumericValue_TO_AR2.md).

## Summary

Bidirectional bridge from the VT input field to an AD2 plug (bitmask) without its own persistence.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

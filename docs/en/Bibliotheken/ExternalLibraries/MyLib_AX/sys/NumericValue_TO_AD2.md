# NumericValue_TO_AD2

![NumericValue_TO_AD2_network](./NumericValue_TO_AD2_network.svg)

* * * * * * * * * *

## Introduction

`NumericValue_TO_AD2` reads a VT input field (CInputNumber) and passes the value bidirectionally as an AD2 adapter (DWORD/bitmask, `VALUEO`) to a consumer (e.g. `AOption_AD_DEMUX_N.K`). The block does **not** store anything itself: the consumer reports the bitmask it actually accepted back over the same AD2 plug, and the echo is written back to the VT field.

![NumericValue_TO_AD2](NumericValue_TO_AD2.svg)

## Interface

### Input variables

- `u16ObjId` (UINT, default `ID_NULL`): ObjectID of the VT input field.

### Adapters

- **Plug `VALUEO`** (`adapter::types::bidirectional::AD2`): bitmask to the consumer and echo back.

The block has no event ports of its own.

## How it works

1. `NumericValue_IDA` (`QI`=TRUE) reads the VT input field.
2. `AD_TO_AD2` converts the value into the bidirectional AD2 adapter, which is output on `VALUEO`.
3. The echo reported back by the consumer appears at `AD_TO_AD2.AD_OUT`, is converted to UDINT via `AD_TO_AUDI`, and written to the VT field under `u16ObjId` by `Q_NumericValue_AUDI` (`u32NewValue`).

## Technical notes

- DWORD/bitmask counterpart of [`NumericValue_TO_AUI2`](./NumericValue_TO_AUI2.md) - for `AOption_AD_DEMUX_N` (multi-select) instead of `AOption_AUI_DEMUX_N` (single-select).
- No `AD_TO_AUI` intermediate step is needed, since AD2 is already a DWORD.
- Pure VT-to-AD2 bridge without a storage FB; compare [`NumericValue_TO_AR2`](./NumericValue_TO_AR2.md).

## Summary

Bidirectional bridge from the VT input field to an AD2 plug (bitmask) without its own persistence.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

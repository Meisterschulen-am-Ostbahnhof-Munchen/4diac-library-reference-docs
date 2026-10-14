# OPC_TO_AR2

![OPC_TO_AR2_network](./OPC_TO_AR2_network.svg)

* * * * * * * * * *

## Introduction

`OPC_TO_AR2` is the pure OPC-UA sibling of [`NumericValue_TO_AR2`](./NumericValue_TO_AR2.md) - no VT input field involved. A remotely-written value is subscribed via `AR_SUBSCRIBE_1` and forwarded to a consumer as an AR2 adapter (`VALUEO`), e.g. `CALIBRATE.ZERO`/`.SPAN`. Whatever the consumer echoes back (current/persisted value) is reported back to the same remote address via `AR_PUBLISH_1`.

![OPC_TO_AR2](OPC_TO_AR2.svg)

## Interface and initialization

- Events: `INIT` (EInit, input) and `INITO` (EInit, output, comes from `AR_PUBLISH_1.INITO`).
- Data: `QI` (BOOL, default `TRUE`) as well as `ID_READ` and `ID_WRITE` (WSTRING) as inputs, `QO` (BOOL) as output; adapter plug `VALUEO` (`AR2`).
- Initialization: `INIT` → `AR_SUBSCRIBE_1.INIT`, `AR_SUBSCRIBE_1.INITO` → `AR_PUBLISH_1.INIT`, `AR_PUBLISH_1.INITO` → `INITO` and → `AND_QO.REQ`.
- Quality: `QI` → `AR_SUBSCRIBE_1.QI` and `AR_PUBLISH_1.QI` (no fixed `QI`=TRUE any more); `AR_SUBSCRIBE_1.QO` → `AND_QO.IN1`, `AR_PUBLISH_1.QO` → `AND_QO.IN2` (`AND_BOOL_2`), `AND_QO.OUT` → `QO`.

## Technical notes

- `AR_PUBLISH_1` is triggered by `AR_SUBSCRIBE_1.INITO` so the boot value is published immediately, not only on the first change.

## Summary

Purely remote (OPC-UA) driven AR2 bridge with no VT involvement at all - for consumers parameterized exclusively from a control room.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

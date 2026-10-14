# NumericValue_TO_AR2_OPC

![NumericValue_TO_AR2_OPC_network](./NumericValue_TO_AR2_OPC_network.svg)

* * * * * * * * * *

## Introduction

`NumericValue_TO_AR2_OPC` is the VT+OPC-UA sibling of [`NumericValue_TO_AR2`](./NumericValue_TO_AR2.md) (VT only) and [`OPC_TO_AR2`](./OPC_TO_AR2.md) (OPC only): both the VT input field and a remotely-written OPC-UA value (`AR_SUBSCRIBE_1`) feed `AR_LAST_2` - "last writer wins". The result goes to the consumer as an AR2 adapter (`VALUEO`); whatever the consumer echoes back is reported both ways: VT display (`Q_NumericValue_PHYSA`) and OPC-UA publish (`AR_PUBLISH_1`).

![NumericValue_TO_AR2_OPC](NumericValue_TO_AR2_OPC.svg)

## Function blocks used

- **NumericValue_PHYSA**: VT input field, local value.
- **AR_SUBSCRIBE_1**: subscribes to a remotely-written REAL value under `ID_READ`.
- **AR_LAST_2**: merges both sources, last writer wins.
- **AR_TO_AR2**: converts the merged plain-AR value into the bidirectional AR2 adapter for the consumer.
- **AR_SPLIT_2**: splits the consumer's echo to the VT display and the OPC publish.
- **AR_PUBLISH_1**: publishes the current/persisted value under `ID_WRITE` (triggered by `AR_SUBSCRIBE_1.INITO`, so the initial value is published immediately too).

## Technical notes

- Merge strategy identical to the VT/web merge already proven in `RampLimitFS_TO_logiBUS_QDA_PWM_OPC.SUB` (per the source comment).

## Event and quality ports (INIT/INITO, QI/QO)

The block has the data input `QI` (`BOOL`, default `TRUE`, Quality Input - TRUE enables the service) and the data output `QO` (`BOOL`, Quality Output - TRUE if the service was executed successfully). `QI` is passed to `NumericValue_PHYSA`, `AR_SUBSCRIBE_1`, `AR_PUBLISH_1` and `Q_NumericValue_PHYSA`. `QO` is the output of the instance `AND_QO` (`iec61131::booleanOperators::AND_BOOL_4`), which combines `Q_NumericValue_PHYSA.QO`, `NumericValue_PHYSA.QO`, `AR_SUBSCRIBE_1.QO` and `AR_PUBLISH_1.QO`.

In addition, the block has the event input `INIT` and the event output `INITO` (both `EInit`). Initialization runs as a chain: `INIT` → `NumericValue_PHYSA.INIT`, `NumericValue_PHYSA.INITO` → `AR_SUBSCRIBE_1.INIT`, `AR_SUBSCRIBE_1.INITO` → `AR_PUBLISH_1.INIT`, `AR_PUBLISH_1.INITO` → `Q_NumericValue_PHYSA.INIT`, `Q_NumericValue_PHYSA.INITO` → `INITO` (and → `AND_QO.REQ`).

## Summary

VT and OPC-UA input for the same AR2 consumer, with last-writer-wins merging and echo feedback on both paths.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

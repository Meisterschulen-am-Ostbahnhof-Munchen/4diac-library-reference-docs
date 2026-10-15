# NumericValue_TO_AR2

![NumericValue_TO_AR2_network](./NumericValue_TO_AR2_network.svg)

* * * * * * * * * *

## Introduction

`NumericValue_TO_AR2` reads a VT input field (`NumericValue_PHYSA`) and forwards the value bidirectionally as an AR2 adapter (`VALUEO`) to a consumer - unlike [`INI_IN_AND_STORE_AR2`](./INI_IN_AND_STORE_AR2.md), this block itself stores **nothing**: persistence is the consumer's responsibility (e.g. `AR_CALIBRATE.Y_Offset`/`.Y_Scale`, or directly an `INI_AR2`). Whatever the consumer echoes back over the same AR2 plug is written back to the VT field - even at boot, so the input field and the actually active value stay in sync.

![NumericValue_TO_AR2](NumericValue_TO_AR2.svg)

## Technical notes

- Pure VT-to-AR2 bridge: no storage FB in the network, unlike the INI/NVS family.
- Historical note: per the source comment, renamed from `INI_IN_AND_STORE_AR2` when the block changed from a self-contained storage function to a pure bridge.

## Quality ports (QI/QO)

The block has the data input `QI` (`BOOL`, default `TRUE`, Quality Input - TRUE enables the service) and the data output `QO` (`BOOL`, Quality Output - TRUE if the service was executed successfully). `QI` is passed on to `Q_NumericValue_PHYSA.QI` and `NumericValue_PHYSA.QI`; their `QO` outputs are combined in `AND_QO` (`iec61131::booleanOperators::AND_BOOL_2`, `Q_NumericValue_PHYSA.QO` → `IN1`, `NumericValue_PHYSA.QO` → `IN2`) into the output `QO`.

In addition, the block has the events `INIT` (input, `EInit`, initialization request) and `INITO` (output, `EInit`, initialization confirmation): Init chain: `INIT` → `Q_NumericValue_PHYSA.INIT`, `Q_NumericValue_PHYSA.INITO` → `NumericValue_PHYSA.INIT`, `NumericValue_PHYSA.INITO` → `INITO` and `AND_QO.REQ`.

## Summary

Bidirectional VT-input-field-to-AR2 bridge with no storage of its own - the consumer persists, this block only displays and forwards.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

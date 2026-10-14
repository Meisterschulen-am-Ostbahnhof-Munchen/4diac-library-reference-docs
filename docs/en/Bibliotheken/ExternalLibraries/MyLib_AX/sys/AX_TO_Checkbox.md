# AX_TO_Checkbox

![AX_TO_Checkbox_network](./AX_TO_Checkbox_network.svg)

* * * * * * * * * *

## Introduction

`AX_TO_Checkbox` is the counterpart of [`Checkbox_IDA_TO_AX`](./Checkbox_IDA_TO_AX.md): it writes a level (`AX_IN`, 0/1) to a VT checkbox (ECU → VT instead of VT → ECU). Internally, `AX_TO_AD` (`adapter::conversion::unidirectional::AX_TO_AUDI`) converts the level into a UDINT value, which `Q_NumericValue` (`isobus::UT::Q::Q_NumericValue_AUDI`) sends to the VT via `u32NewValue`.

![AX_TO_Checkbox](AX_TO_Checkbox.svg)

## Interface

### Event ports

- **Input `INIT`** (`EInit`): initialization request - to `Q_NumericValue.INIT`.
- **Output `INITO`** (`EInit`): initialization confirmation - from `Q_NumericValue.INITO`.

### Input variables

- `QI` (BOOL, default `TRUE`): Quality Input - TRUE to enable the service (to `Q_NumericValue.QI`).
- `u16ObjId` (UINT, default `ID_NULL`): ObjectID of the VT NumberVariable behind the checkbox.

### Output variables

- `QO` (BOOL): Quality Output - TRUE if the service was executed successfully (from `Q_NumericValue.QO`).

### Adapters

- **Socket `AX_IN`** (`adapter::types::unidirectional::AX`): level to be written (0/1).

Apart from `INIT`/`INITO`, the block has no event ports of its own; writing is triggered by the adapter `AX_IN`.

## How it works

1. `AX_IN` is converted into a UDINT value via `AX_TO_AD`.
2. `Q_NumericValue` writes this value to the VT node under `u16ObjId`. According to the source comment, it is rewritten on every value change of `AX_IN`.
3. `INIT` → `Q_NumericValue.INIT`, `Q_NumericValue.INITO` → `INITO`, `QI` → `Q_NumericValue.QI`, `Q_NumericValue.QO` → `QO`.

## Technical notes

- `u16ObjId` is the ObjectID of the **CNumberVariable** that the checkbox references as its value store - **not** the ObjectID of the checkbox (`CInputBoolean`) itself. The VT expects value changes under the NumberVariable ID (analogous to `Checkbox_IDA_TO_AX`).
- Instance uniqueness: `Q_NumericValue` allows only one instance per target ObjectID in the entire project.

## Summary

Writes an AX level to a VT checkbox via its associated NumberVariable.

## 🛠️ Related Exercises

- [Uebung_094b5_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b5_AX/)

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

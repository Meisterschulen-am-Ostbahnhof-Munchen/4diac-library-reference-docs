# Checkbox_TO_AD2_2

![Checkbox_TO_AD2_2_network](./Checkbox_TO_AD2_2_network.svg)

* * * * * * * * * *

## Introduction

`Checkbox_TO_AD2_2` combines two VT checkboxes into a bitmask and passes it bidirectionally as an AD2 adapter (`K`) to a consumer (e.g. `AOption_AD_DEMUX_2.K`). Checkbox 1 corresponds to bit 0, checkbox 2 to bit 1. The bitmask reported back by the consumer (the actually active one) is written back to the two checkboxes.

![Checkbox_TO_AD2_2](Checkbox_TO_AD2_2.svg)

## Interface

### Event inputs

- `INIT` (`EInit`): initialization request.

### Event outputs

- `INITO` (`EInit`): initialization confirm.

### Input variables

- `QI` (BOOL, default `TRUE`): quality input, TRUE enables the service.
- `u16ObjId_1` (UINT, default `ID_NULL`): ObjectID of checkbox 1 (bit 0).
- `u16ObjId_2` (UINT, default `ID_NULL`): ObjectID of checkbox 2 (bit 1).

### Output variables

- `QO` (BOOL): quality output, AND combination (`AND_QO`, `AND_BOOL_4`) of the `QO` outputs of the two `Checkbox_IDA_TO_AX` and the two `AX_TO_Checkbox` instances.

### Adapters

- **Plug `K`** (`adapter::types::bidirectional::AD2`): bitmask to the consumer and echo back.

Initialization runs as a chain through the four sub-blocks (see below).

## How it works

Forward direction (VT → consumer):

1. Two instances of [`Checkbox_IDA_TO_AX`](./Checkbox_IDA_TO_AX.md) each read one checkbox and deliver an AX level (`PERMIT_OUT`).
2. `ASSEMBLE_AD_FROM_AX` assembles the levels into a DWORD (`BIT_00` and `BIT_01`).
3. `AD_TO_AD2` converts it into the bidirectional AD2 adapter, which is output on `K`.

Reverse direction (consumer → VT):

1. The echo reported back via `K` appears at `AD_TO_AD2.AD_OUT`.
2. `SPLIT_AD_INTO_AX` splits the DWORD into `BIT_00` and `BIT_01`.
3. Two instances of [`AX_TO_Checkbox`](./AX_TO_Checkbox.md) write the bits to the respective checkbox.

Initialization and quality:

1. `INIT` → `Checkbox_IDA_TO_AX.INIT`; `Checkbox_IDA_TO_AX.INITO` → `Checkbox_IDA_TO_AX_1.INIT`; `Checkbox_IDA_TO_AX_1.INITO` → `AX_TO_Checkbox.INIT`; `AX_TO_Checkbox.INITO` → `AX_TO_Checkbox_1.INIT`; `AX_TO_Checkbox_1.INITO` → `INITO` and → `AND_QO.REQ`.
2. `QI` → `.QI` of all four sub-blocks; their `QO` → `AND_QO.IN1` to `AND_QO.IN4` (`Checkbox_IDA_TO_AX`, `Checkbox_IDA_TO_AX_1`, `AX_TO_Checkbox`, `AX_TO_Checkbox_1`); `AND_QO.OUT` → `QO`.

`u16ObjId_1` and `u16ObjId_2` each go to both the `Checkbox_IDA_TO_AX` and the `AX_TO_Checkbox` instance of the corresponding checkbox.

## Technical notes

- Extracted from `Uebung_094b5_AX` for reuse; designed for N=2 bits/checkboxes, analogous to `AOption_AD_DEMUX_2`.
- Does not store anything itself; persistence is the consumer's responsibility.

## Summary

Bundles two VT checkboxes as a bitmask onto a bidirectional AD2 plug and mirrors the consumer echo back to the checkboxes.

## 🛠️ Related Exercises

- [Uebung_094b5b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b5b_AX/)

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

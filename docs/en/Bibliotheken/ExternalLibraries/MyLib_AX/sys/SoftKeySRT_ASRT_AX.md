# SoftKeySRT_ASRT_AX

![SoftKeySRT_ASRT_AX_network](./SoftKeySRT_ASRT_AX_network.svg)

* * * * * * * * * *

## Introduction

`SoftKeySRT_ASRT_AX` bundles 3 softkeys (set/reset/toggle) and a `GreenWhiteBackground1_AX` (on the toggle softkey) behind a single `ASRT_AX` plug (bidirectional: set/reset/toggle go out, state comes in). A pure HMI block with no OPC-UA involvement - the SR+toggle counterpart of [`SoftKeySR_ASR_AX`](./SoftKeySR_ASR_AX.md).

![SoftKeySRT_ASRT_AX](SoftKeySRT_ASRT_AX.svg)

## Function blocks used

- **SoftKey_SET / SoftKey_RESET / SoftKey_TOGGLE** (`isobus::UT::io::Softkey::Softkey_IE`, `InputEvent=SK_RELEASED`): the 3 physical softkeys.
- **ASRT_3EVENTS_TO_SRT** (`adapter::conversion::unidirectional::ASRT_3EVENTS_TO_SRT`): converts the 3 softkey events into an ASRT adapter event triple.
- **SPLIT** (`adapter::events::bidirectional::ASRT_TO_ASRT_AX_SPLIT`): splits the bidirectional `ASRT_AX` traffic to `OUT` and to the background element.
- **GreenWhiteBackground_AX** (SubApp, type `MyLib::sys::GreenWhiteBackground1_AX`): status color on the toggle softkey.
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_4`): combines the `QO` outputs of the inner blocks into `QO`.

## Initialization and Quality

Interface (quality pattern): event input `INIT` and event output `INITO` (each `EInit`), data input `QI` (`BOOL`, default `TRUE`) and data output `QO` (`BOOL`). Initialization: `INIT` → `SoftKey_SET.INIT`; `SoftKey_SET.INITO` → `SoftKey_RESET.INIT`; `SoftKey_RESET.INITO` → `SoftKey_TOGGLE.INIT`; `SoftKey_TOGGLE.INITO` → `GreenWhiteBackground_AX.INIT`; `GreenWhiteBackground_AX.INITO` → `INITO` and → `AND_QO.REQ`. Quality: `QI` → `SoftKey_SET.QI`, `SoftKey_RESET.QI`, `SoftKey_TOGGLE.QI`, `GreenWhiteBackground_AX.QI`; `SoftKey_SET.QO`, `SoftKey_RESET.QO`, `SoftKey_TOGGLE.QO`, `GreenWhiteBackground_AX.QO` → `AND_QO.IN1` to `AND_QO.IN4` (in this order); `AND_QO.OUT` → `QO` (`AND_QO`: `iec61131::booleanOperators::AND_BOOL_4`).

## Summary

Reusable set/reset/toggle-with-status block, bundled behind an `ASRT_AX` adapter - the HMI counterpart of [`Uebung_010e_PC_A_OPC_Adapter`](./Uebung_010e_PC_A_OPC_Adapter.md)/[`Uebung_010e_PC_B_OPC_Adapter`](./Uebung_010e_PC_B_OPC_Adapter.md).

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

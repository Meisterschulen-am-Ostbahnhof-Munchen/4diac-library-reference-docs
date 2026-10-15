# SoftKeySR_ASR_AX

![SoftKeySR_ASR_AX_network](./SoftKeySR_ASR_AX_network.svg)

* * * * * * * * * *

## Introduction

`SoftKeySR_ASR_AX` bundles 2 softkeys (set/reset) and a `GreenWhiteBackground1_AX` (on the set softkey) behind a single `ASR_AX` plug (bidirectional: set/reset go out, state comes in). A pure HMI block with no OPC-UA involvement.

![SoftKeySR_ASR_AX](SoftKeySR_ASR_AX.svg)

## Interface

- **Event input**: `INIT` (`EInit`, Initialization Request)
- **Event output**: `INITO` (`EInit`, Initialization Confirm)
- **Data inputs**: `QI` (`BOOL`, default `TRUE`, Quality Input), `u16ObjId_SET`, `u16ObjId_RESET` (`UINT`, default `ID_NULL`)
- **Data output**: `QO` (`BOOL`, Quality Output)
- **Plug**: `OUT` (`adapter::types::bidirectional::ASR_AX`)

## Function blocks used

- **SoftKey_SET / SoftKey_RESET** (`isobus::UT::io::Softkey::Softkey_IE`, `InputEvent=SK_RELEASED`): the two physical softkeys.
- **ASR_2EVENTS_TO_SR** (`adapter::conversion::unidirectional::ASR_2EVENTS_TO_SR`): converts the 2 softkey events into an ASR adapter event pair.
- **SPLIT** (`adapter::events::bidirectional::ASR_TO_ASR_AX_SPLIT`): splits the bidirectional `ASR_AX` traffic to `OUT` and to the background element.
- **GreenWhiteBackground_AX** (SubApp, type `MyLib::sys::GreenWhiteBackground1_AX`): status color on the set softkey.
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_3`): ANDs the `QO` outputs of `SoftKey_SET`, `SoftKey_RESET` and `GreenWhiteBackground_AX` into the block output `QO`.

## Initialization and status

- Initialization chain: `INIT` → `SoftKey_SET.INIT`, `SoftKey_SET.INITO` → `SoftKey_RESET.INIT`, `SoftKey_RESET.INITO` → `GreenWhiteBackground_AX.INIT`, `GreenWhiteBackground_AX.INITO` → `INITO` and → `AND_QO.REQ`
- `QI` → `SoftKey_SET.QI`, `SoftKey_RESET.QI` and `GreenWhiteBackground_AX.QI`
- `SoftKey_SET.QO` → `AND_QO.IN1`, `SoftKey_RESET.QO` → `AND_QO.IN2`, `GreenWhiteBackground_AX.QO` → `AND_QO.IN3`; `AND_QO.OUT` → `QO`

## Summary

Reusable set/reset-with-status block, bundled behind an `ASR_AX` adapter - the HMI counterpart of [`SoftKeySR_PC_A_OPC_Adapter`](./SoftKeySR_PC_A_OPC_Adapter.md)/[`SoftKeySR_PC_B_OPC_Adapter`](./SoftKeySR_PC_B_OPC_Adapter.md).

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

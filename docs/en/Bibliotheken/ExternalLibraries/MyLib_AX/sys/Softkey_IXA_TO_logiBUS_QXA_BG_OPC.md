# Softkey_IXA_TO_logiBUS_QXA_BG_OPC

![Softkey_IXA_TO_logiBUS_QXA_BG_OPC_network](./Softkey_IXA_TO_logiBUS_QXA_BG_OPC_network.svg)

* * * * * * * * * *

## Introduction

`Softkey_IXA_TO_logiBUS_QXA_BG_OPC` is the SoftKey counterpart to [`Button_IXA_TO_logiBUS_QXA_BG_OPC`](./Button_IXA_TO_logiBUS_QXA_BG_OPC.md): A VT SoftKey and an OPC UA remote command are each converted into set/reset events via edge detection, combined in a common latch, and control a single digital logiBUS output—including VT status display and OPC UA echo. One click on the SoftKey or a remote command turns the output ON, a second click turns it OFF (click-toggle behavior).



## Function Blocks (FBs) Used

### Sub-Blocks: Softkey_IXA_TO_logiBUS_QXA_BG_OPC

- **Type**: SubAppType
- **Internal FBs Used**:

- **Softkey_IXA**: `isobus::UT::io::Softkey::Softkey_IXA` (`QI=TRUE`) — VT SoftKey, `u16ObjId` identifies the SoftKey.

- **AX_ASR_RF_TRIG_BT** / **AX_ASR_RF_TRIG_OPC**: each `adapter::events::unidirectional::AX_ASR_RF_TRIG` — convert the rising/falling edge of the SoftKey or the OPC UA Subscribe value into a Set/Reset event pair (ASR).

- **ASR_MERGE_2**: `adapter::events::unidirectional::ASR_MERGE_2` — combines the two ASR sources (SoftKey, OPC UA).

- **ASR_AX_SR**: `adapter::events::unidirectional::ASR_AX_SR` — Set/Reset Latch, creates the latched output state from the combined ASR signal.

- **AX_SPLIT_3**: `adapter::events::unidirectional::AX_SPLIT_3` — distributes the latch output to three destinations.

- **logiBUS_QXA**: `logiBUS::io::DQ::logiBUS_QXA` (`QI=TRUE`) — physical digital output.

- **GreenWhiteBackground1_AX** (SubApp, `MyLib::sys`): VT background color matching the output state.

- **AX_SUBSCRIBE_1** / **AX_PUBLISH_1**: each `adapter::net::AX_SUBSCRIBE_1`/`AX_PUBLISH_1` (`QI=TRUE`) — OPC UA read/write access.

- **Functionality**: Identical to the latching structure of `Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING`, but with `Softkey_IXA` instead of `Button_IXA` as the local input source.

## Program Flow and Connections

1. `Softkey_IXA.IN` (adapter) → `AX_ASR_RF_TRIG_BT.QI` → `AX_ASR_RF_TRIG_BT.Q` → `ASR_MERGE_2.IN2`.

2. `AX_SUBSCRIBE_1.OUT` → `AX_ASR_RF_TRIG_OPC.QI` → `AX_ASR_RF_TRIG_OPC.Q` → `ASR_MERGE_2.IN1`.
3. `ASR_MERGE_2.OUT` → `ASR_AX_SR.S_R`.
4. `ASR_AX_SR.Q` → `AX_SPLIT_3.IN` → `AX_SPLIT_3.OUT1` → `logiBUS_QXA.OUT`, `AX_SPLIT_3.OUT2` → `GreenWhiteBackground1_AX.DI1`, `AX_SPLIT_3.OUT3` → `AX_PUBLISH_1.IN`.
5. Initialization: `AX_SUBSCRIBE_1.INITO` → `AX_PUBLISH_1.INIT` (hidden connection).

6. Parameters: `u16ObjId` → `Softkey_IXA.u16ObjId` and `GreenWhiteBackground1_AX.u16ObjId`; `Output` → `logiBUS_QXA.Output`; `ID_READ` → `AX_SUBSCRIBE_1.ID`; `ID_WRITE` → `AX_PUBLISH_1.ID`.


## Technical Features

- **Click toggle via ASR latch**: As with `Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING`, the output remains in its last set state after release—unlike a momentary OR gate.

- **Two independent toggle sources**: A soft key and an OPC UA command each trigger a toggle independently; `ASR_MERGE_2` combines both into a common latch input.

## Application Scenarios

- Digital outputs that are to be switched on/off via a soft key or remote command (toggle behavior), with VT status display and OPC UA feedback.


## Comparison with Similar Modules

Structurally identical to [`Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING`](./Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING.md), except that `Softkey_IXA` is used instead of `Button_IXA` as the local control source. Compared to the simpler [`Softkey_IXA_TO_logiBUS_QXA_BG`](./Softkey_IXA_TO_logiBUS_QXA_BG.md) (only local softkey, no OPC UA connection, no latch logic), this module includes the complete edge detection/merge/latch chain as well as OPC UA subscribe/publish functionality.

## Summary

`Softkey_IXA_TO_logiBUS_QXA_BG_OPC` switches a digital logiBUS output via softkey OR remote OPC UA command using a click-toggle method, including VT status color and OPC UA echo.



[`Softkey_IXA_TO_logiBUS_QXA_BG_OPC`] ---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

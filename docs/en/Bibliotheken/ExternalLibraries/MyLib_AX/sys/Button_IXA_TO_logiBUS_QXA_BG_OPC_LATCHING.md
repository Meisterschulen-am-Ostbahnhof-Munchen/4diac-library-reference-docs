# Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING

![Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING_network](./Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING_network.svg)

* * * * * * * * * *

## Introduction

`Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING` is the click-toggle variant (SR latch) of [`Button_IXA_TO_logiBUS_QXA_BG_OPC`](./Button_IXA_TO_logiBUS_QXA_BG_OPC.md): A click on the VT button or an OPC UA remote command switches the output ON, a second click switches it OFF again — regardless of how long the button is pressed. The block is retained in case this click-toggle behavior is needed again; since 2026-09-07, `Button_IXA_TO_logiBUS_QXA_BG_OPC` itself uses a simple momentary latch `AX_OR_2` instead. Both function blocks share the same interface (`u16ObjId`/`Output`/`ID_READ`/`ID_WRITE`) and are interchangeable.

## Function Blocks (FBs) Used

### Sub-Blocks: Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING

- **Type**: SubAppType
- **Internal FBs Used**:

- **Button_IXA**: `isobus::UT::io::Button::Button_IXA` (`QI=TRUE`) — VT button, `u16ObjId` identifies the VT button.

- **AX_ASR_RF_TRIG_BT** / **AX_ASR_RF_TRIG_OPC**: each `adapter::events::unidirectional::AX_ASR_RF_TRIG` — convert the rising/falling edge of the VT button or the OPC UA subscribe value into a Set/Reset event pair (ASR).

- **ASR_MERGE_2**: `adapter::events::unidirectional::ASR_MERGE_2` — combines the two ASR sources (button, OPC UA).

- **ASR_AX_SR**: `adapter::events::unidirectional::ASR_AX_SR` — Set/Reset latch that creates the latched output state (last wins) from the combined ASR signal.

- **AX_SPLIT_3**: `adapter::events::unidirectional::AX_SPLIT_3` — distributes the latch output to three destinations.

- **logiBUS_QXA**: `logiBUS::io::DQ::logiBUS_QXA` (`QI=TRUE`) — Physical digital output.

- **GreenWhiteBackground1_AX** (SubApp, `MyLib::sys`): VT background color matching the output state.

- **AX_SUBSCRIBE_1** / **AX_PUBLISH_1**: each `adapter::net::AX_SUBSCRIBE_1`/`AX_PUBLISH_1` (`QI=TRUE`) — OPC UA read/write access.

**Functionality**: Both switching sources (VT button, OPC UA command) are each converted into set/reset events via `AX_ASR_RF_TRIG`, combined via `ASR_MERGE_2`, and latched in `ASR_AX_SR` — the latch state simultaneously powers the hardware, the VT status color, and the OPC UA echo.

## Program Flow and Connections

1. `Button_IXA.IN` (adapter) → `AX_ASR_RF_TRIG_BT.QI` → `AX_ASR_RF_TRIG_BT.Q` → `ASR_MERGE_2.IN2`.


2. `AX_SUBSCRIBE_1.OUT` → `AX_ASR_RF_TRIG_OPC.QI` → `AX_ASR_RF_TRIG_OPC.Q` → `ASR_MERGE_2.IN1`

3. `ASR_MERGE_2.OUT` → `ASR_AX_SR.S_R` (Set/Reset input of the latch).

4. `ASR_AX_SR.Q` → `AX_SPLIT_3.IN` → `AX_SPLIT_3.OUT1` → `logiBUS_QXA.OUT`, `AX_SPLIT_3.OUT2` → `GreenWhiteBackground1_AX.DI1`, `AX_SPLIT_3.OUT3` → `AX_PUBLISH_1.IN`.

5. Initialization: `AX_SUBSCRIBE_1.INITO` → `AX_PUBLISH_1.INIT` (hidden connection).

6. Parameters: `u16ObjId` → `Button_IXA.u16ObjId` and `GreenWhiteBackground1_AX.u16ObjId`; `Output` → `logiBUS_QXA.Output`; `ID_READ` → `AX_SUBSCRIBE_1.ID`; `ID_WRITE` → `AX_PUBLISH_1.ID`.

## Technical Features

- **Click toggle via ASR latch**: Unlike the momentary `AX_OR_2` variant, this block converts each actuation into a set/reset event, which is latched in `ASR_AX_SR` — the output remains in the last set state after release.

- **Two independent toggle sources**: The VT button and the OPC UA command each trigger a toggle independently; `ASR_MERGE_2` combines both into a common latch input.

- **Identical external interface**: Despite completely different internal logic, the interface is identical to `Button_IXA_TO_logiBUS_QXA_BG_OPC`, so both variants can be instantiated interchangeably.

## Application Scenarios

- Outputs that should be switched on/off with a single click (toggle behavior), instead of only being active while the button is held.


## Comparison with Similar Modules

Compared to [`Button_IXA_TO_logiBUS_QXA_BG_OPC`](./Button_IXA_TO_logiBUS_QXA_BG_OPC.md) (currently: tactile `AX_OR_2`), this module replaces the simple OR operation with a complete edge detection/merge/latch chain (`AX_ASR_RF_TRIG` × 2, `ASR_MERGE_2`, `ASR_AX_SR`) for click-toggle behavior.

## Summary

`Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING` offers the same basic functionality as `Button_IXA_TO_logiBUS_QXA_BG_OPC`, but with click-toggle behavior instead of tactile behavior—directly interchangeable with an identical external interface.


---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

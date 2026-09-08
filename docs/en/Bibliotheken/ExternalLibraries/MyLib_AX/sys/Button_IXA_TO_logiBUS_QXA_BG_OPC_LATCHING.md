# Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING

![Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING_network](./Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING_network.svg)

* * * * * * * * * *

## Introduction

`Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING` replaces the simple OR gate (`AX_OR_2`) of [`Button_IXA_TO_logiBUS_QXA_BG_OPC`](./Button_IXA_TO_logiBUS_QXA_BG_OPC.md) with an edge-detection/merge/latch chain (`AX_ASR_RF_TRIG` × 2, `ASR_MERGE_2`, `ASR_AX_SR`). This is **not** a true click-toggle: `AX_ASR_RF_TRIG` maps the rising edge (press) to `SET` and the falling edge (release) to `RESET`, so with only one source active the block behaves exactly like the momentary `AX_OR_2` variant (ON only while held). The difference only shows up once the VT button and the OPC UA command overlap in time: unlike a true OR gate (which stays ON as long as either source is active), the last edge to arrive always wins here, regardless of which source it came from — e.g. if the OPC UA side sends a release while the VT button is still held, the output switches OFF immediately anyway. The block is retained for use cases that need exactly this last-wins behavior between two sources. Both function blocks share the same interface (`u16ObjId`/`Output`/`ID_READ`/`ID_WRITE`) and are interface-compatible — their switching behavior differs only in this overlap case.

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

- **Last-wins via ASR latch, not a real toggle**: Each actuation produces a set (press) or reset (release) event per source; `ASR_AX_SR` latches the combined result. With only one source active, the behavior is purely momentary (ON only while held) — identical to the `AX_OR_2` variant. The difference only shows up once both sources overlap: whichever edge (set or reset, from either source) arrives last determines the state, rather than a continuous OR.

- **Two independent last-wins sources**: The VT button and the OPC UA command each independently deliver set/reset events; `ASR_MERGE_2` combines both into a common latch input without prioritizing either source.

- **Interface-compatible external interface**: The interface is identical to `Button_IXA_TO_logiBUS_QXA_BG_OPC`, so both variants can be instantiated interchangeably. The switching behavior is only identical with a single active source — with overlapping use of both sources it differs (last-wins vs. a true OR gate).

## Application Scenarios

- Outputs with two independent switching sources (e.g. a local VT button AND a remote OPC UA command) where whichever action arrives last (press or release, from either source) should win, instead of one permanently active source blocking the other — which is exactly what a true OR gate would do.


## Comparison with Similar Modules

Compared to [`Button_IXA_TO_logiBUS_QXA_BG_OPC`](./Button_IXA_TO_logiBUS_QXA_BG_OPC.md) (currently: tactile `AX_OR_2`), this module replaces the simple OR operation with an edge-detection/merge/latch chain (`AX_ASR_RF_TRIG` × 2, `ASR_MERGE_2`, `ASR_AX_SR`). With only one source active the behavior is identical (momentary); the difference only shows up once both sources overlap (last-wins instead of OR).

## Summary

With only one source active, `Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING` offers the same momentary basic functionality as `Button_IXA_TO_logiBUS_QXA_BG_OPC` — the difference is last-wins behavior for overlapping VT/OPC UA commands, not a click-toggle. Both blocks are interface-compatible, but not behaviorally identical in every scenario.


---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

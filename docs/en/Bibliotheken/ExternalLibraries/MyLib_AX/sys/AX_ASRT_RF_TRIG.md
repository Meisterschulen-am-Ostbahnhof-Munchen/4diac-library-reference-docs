# AX_ASRT_RF_TRIG

![AX_ASRT_RF_TRIG_network](./AX_ASRT_RF_TRIG_network.svg)

* * * * * * * * * *

## Introduction

`AX_ASRT_RF_TRIG` detects rising and falling edges of a `AX` signal and combines them as a `ASRT` adapter (Set/Reset, Toggle remains unused). Instead of a new low-level `FBType`, the function block is constructed as a composite of the existing `AX_ASR_RF_TRIG` (which returns a `ASR` from an edge) and `ASRT_SR_AE_TO_SRT` (which combines a `ASR` with an optional AE toggle event to create a `ASRT`).



## Function Blocks (FBs) Used

### Sub-Blocks: AX_ASRT_RF_TRIG

- **Type**: SubAppType

- **Internal FBs Used**:

- **AX_ASR_RF_TRIG_1**: `adapter::events::unidirectional::AX_ASR_RF_TRIG` — rising edge at `QI` → SET, falling edge → RESET, bundled as `ASR`.

- **ASRT_SR_AE_TO_SRT_1**: `adapter::conversion::unidirectional::ASRT_SR_AE_TO_SRT` — takes `ASR` as `SR_IN` and bundles it to `ASRT_OUT`; The second socket (`TOGGLE_IN`, a single AE event) remains unwired and never fires, as a pure edge signal does not allow for the derivation of an independent third event.

- **Functionality**: `AX_ASR_RF_TRIG_1` generates a `ASR` from the edge of `QI`. This `SR_IN` is directly fed into `ASRT_SR_AE_TO_SRT_1`, whose `ASRT_OUT` then serves the plug `Q`.

## Program Flow and Connections

1. `QI` (Socket) → `AX_ASR_RF_TRIG_1.QI`.


2. `AX_ASR_RF_TRIG_1.Q` → `ASRT_SR_AE_TO_SRT_1.SR_IN`.

3. `ASRT_SR_AE_TO_SRT_1.ASRT_OUT` → `Q` (Plug).

## Technical Features

- **Composite instead of a new low-level component**: Reuses two existing components instead of implementing a new `FBType`.

- **TOGGLE socket remains unused**: `ASRT_SR_AE_TO_SRT_1.TOGGLE_IN` is never wired and therefore never fires—the component only provides SET/RESET, never a true toggle event.


## Application Scenarios

- As one of several `ASRT` sources for a `ASRT_MERGE_2`, e.g., in `Uebung_232_AX`.

## Summary

`AX_ASRT_RF_TRIG` delivers a `ASRT` adapter signal (Set/Reset only, Toggle unused) from a single edge detection and can therefore be directly connected to devices that expect a `ASRT` socket.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de ](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

# SF_ResetButton

![SF_ResetButton_ecc](./SF_ResetButton_ecc.svg)

* * * * * * * * * *

## Introduction

`SF_ResetButton` is the PLCopen safety block `SF_ResetButton` (PLCopen TC5 Safety Part 1 v2.10, chapter 6.1) with trailing-edge reset. It monitors a reset button: a valid reset exists when the button is pressed (rising edge) and released again (falling edge) after a time between `TrailingMinimum` and `TrailingMaximum`. `ResetOut` then outputs a single pulse.

![SF_ResetButton](SF_ResetButton.svg)

## Interface Structure

### **Event Inputs**

- **REQ**: cyclic evaluation request
    - **Associated data**: `ResetRequested`, `ResetIn`, `TrailingMinimum`, `TrailingMaximum`

### **Event Outputs**

- **CNF**: confirmation of the requested service
    - **Associated data**: `Ready`, `ResetOut`, `Error`, `DiagCode`
- **EResetOut**: dedicated event that fires once in exactly the cycle in which a valid trailing-edge reset is detected (same cycle as `ResetOut = TRUE`)

### **Data Inputs**

- **ResetRequested** (BOOL): from the paired FB (`ResetRequest` output). TRUE = monitor `ResetIn`, FALSE = inactive (default TRUE)
- **ResetIn** (BOOL): reset button input. FALSE = released, TRUE = actuated (default FALSE)
- **TrailingMinimum** (TIME): minimum actuation time (absolute minimum 100 ms). Shorter actuations are not accepted as reset (default `T#350ms`)
- **TrailingMaximum** (TIME): maximum actuation time. Longer actuations are not accepted as reset (default `T#2s`)

### **Data Outputs**

- **Ready** (BOOL): FB active, outputs valid
- **ResetOut** (BOOL): one-cycle pulse when a valid trailing-edge reset has been detected
- **Error** (BOOL): TRUE while an error state exists (see `DiagCode`)
- **DiagCode** (WORD): FB-specific status/error code (PLCopen Safety Part 1, chap. 6.1.6)

### **Adapters**

- **minTimeOut** (plug, `iec61499::events::ATimeOut`): timer for `TrailingMinimum`
- **maxTimeOut** (plug, `iec61499::events::ATimeOut`): timer for `TrailingMaximum`

## Functionality

The block is a basic FB with an ECC. Internal variables: `xPrevResetIn` (value of `ResetIn` in the previous cycle for edge detection), `xMinOk` (TRUE as soon as `TrailingMinimum` has elapsed since the rising edge) and `xParamOk` (`TrailingMinimum <= TrailingMaximum` and `TrailingMinimum >= T#100ms`).

- **Activation**: with `ResetRequested = FALSE` the block stays in state `sIdle`. With `ResetRequested = TRUE` it moves to `sWaitRTrig`.
- **`sWaitRTrig`**: if the parameters are invalid, `sParamError` follows. If `ResetIn` was already pressed before activation (`ResetIn AND xPrevResetIn`), `sResetError` follows. A fresh rising edge (`ResetIn AND NOT xPrevResetIn`) leads to `sArmTimers`.
- **`sArmTimers`**: sets `minTimeOut.DT := TrailingMinimum` and `maxTimeOut.DT := TrailingMaximum`, clears `xMinOk`, starts both timers (`minTimeOut.START`, `maxTimeOut.START`) and moves unconditionally to `sWaitFTrig`.
- **`sWaitFTrig`**: if `minTimeOut` expires, `xMinOk` is set via `sMinElapsed`. If `maxTimeOut` expires, `sErrorMax` follows. If the button is released (`NOT ResetIn AND xPrevResetIn`), the state `sResetDetected` follows when `xMinOk` is set, otherwise `sErrorMin`.
- **`sResetDetected`**: `ResetOut := TRUE`, `EResetOut` is output, both timers are stopped; then unconditionally back to `sWaitRTrig`.
- **Error states** (`sParamError`, `sResetError`, `sErrorMin`, `sErrorMax`): `Error := TRUE` with the respective `DiagCode`. From `sErrorMin`/`sErrorMax` a new rising edge leads back to `sArmTimers`, from `sResetError` releasing the button leads to `sWaitRTrig`. In every state, `ResetRequested = FALSE` leads back to `sIdle`.

## Technical Details

- `EResetOut` is a non-literal IEC 61499 extension of the PLCopen interface: a dedicated event lets consumers wire the reset pulse directly instead of deriving it from `CNF` and `ResetOut` through an external gate (version 1.1)
- Structure following the pattern of `sequence_T_05` (ECC + `ATimeOut` plugs)
- `ResetOut` and `EResetOut` are active in exactly one cycle only
- The diagnostic codes come from the constants `DiagCodes` and `ResetButtonDiagCodes` (`logiBUS::safety::PLCopen::const`)

## State Overview

| State | DiagCode | Meaning |
| ----- | -------- | ------- |
| xSTART | - | initial state, moves unconditionally to `sIdle` |
| sIdle | 0000 | FB not active |
| sParamError | C000 | parameter error: `TrailingMinimum > TrailingMaximum` or `TrailingMinimum < 100 ms` |
| sResetError | C001 | reset error: `ResetIn` was already TRUE when `ResetRequested` became TRUE |
| sWaitRTrig | 83E2 | active, waits for the button to be actuated (rising edge) |
| sArmTimers | 83F2 | one-shot: (re)start both timers, then `sWaitFTrig` |
| sWaitFTrig | 83F2 | button pressed, waits for release (falling edge) |
| sMinElapsed | 83F2 | one-shot: set `xMinOk`, then back to `sWaitFTrig` |
| sErrorMin | C3F0 | error: released before `TrailingMinimum` elapsed, waits for a new rising edge |
| sErrorMax | C3E0 | error: `TrailingMaximum` elapsed before release, waits for a new rising edge |
| sResetDetected | 8000 | valid reset, 1-cycle pulse on `ResetOut` and `EResetOut` |

## Application Scenarios

- Acknowledging a safety function (e.g. after an emergency stop) with a deliberate, time-limited button actuation
- Interplay with a block that requests the acknowledgement via `ResetRequest` and couples to `ResetRequested`

## 🛠️ Related Exercises

- [Uebung_006_SAFE_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006_SAFE_AX/)

## Conclusion

`SF_ResetButton` detects a valid trailing-edge reset according to PLCopen Safety, monitors the minimum and maximum actuation duration with two timers and reports errors via `Error` and `DiagCode`.

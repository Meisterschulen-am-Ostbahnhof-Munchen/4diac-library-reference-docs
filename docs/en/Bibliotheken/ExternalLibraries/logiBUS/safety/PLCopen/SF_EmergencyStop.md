# SF_EmergencyStop

![SF_EmergencyStop_ecc](./SF_EmergencyStop_ecc.svg)

* * * * * * * * * *

## Introduction

`SF_EmergencyStop` is the PLCopen Safety function `SF_EmergencyStop` (Part 1 v2.10, Ch. 6.5) for **monitoring an emergency stop sensor** (`S_EStopIn`). The output `S_EStopOut` only becomes `TRUE` again after `S_EStopIn` is `TRUE` again **and** an acknowledgment has occurred (`S_StartReset`/`S_AutoReset`/`Reset`) - per EN ISO 13849-1:2015 5.2.2 and IEC 60204-1:2016 9.2.3.4.

![SF_EmergencyStop](SF_EmergencyStop.svg)

## Interface Structure

### **Event Inputs**

- **REQ**: Cyclic evaluation request. Associated data: `Activate`, `S_EStopIn`, `S_StartReset`, `S_AutoReset`, `Reset`
- **EReset**: Dedicated reset-edge event in addition to `REQ`, typically fed directly by `SF_ResetButton.EResetOut`. Associated data: `Activate`, `S_EStopIn`

### **Event Outputs**

- **CNF**: Confirmation of the requested service. Associated data: `Ready`, `S_EStopOut`, `SafetyDemand`, `ResetRequest`, `Error`, `DiagCode`

### **Data Inputs**

- **Activate** (`BOOL`, default `FALSE`): Block enable; `FALSE` = inactive (`sIdle`)
- **S_EStopIn** (`BOOL`, default `FALSE`): Emergency stop sensor; `FALSE` = stop engaged (demand), `TRUE` = not engaged
- **S_StartReset** (`BOOL`, default `FALSE`): `TRUE` = auto-acknowledge the first time this block is activated
- **S_AutoReset** (`BOOL`, default `FALSE`): `TRUE` = auto-acknowledge every later recovery of `S_EStopIn`, no `Reset` needed
- **Reset** (`BOOL`, default `FALSE`): Manual reset, typically fed by the paired `SF_ResetButton.ResetOut`

### **Data Outputs**

- **Ready** (`BOOL`): Block active, outputs valid
- **S_EStopOut** (`BOOL`): `TRUE` = no demand, acknowledged, safe to run
- **SafetyDemand** (`BOOL`): `TRUE` while waiting for `S_EStopIn` to recover
- **ResetRequest** (`BOOL`): Feeds the paired `SF_ResetButton.ResetRequested` input
- **Error** (`BOOL`): `TRUE` in an error state, see `DiagCode`
- **DiagCode** (`WORD`, default `16#0000`): Block-specific status/error code (PLCopen Safety Part 1, Ch. 6.5.6)

### **Adapters**

This function block uses no adapters.

## How it works

The block is a basic FB with nine states. The outputs are set in each state by its own algorithm; afterwards `CNF` is fired.

| State | DiagCode | S_EStopOut | SafetyDemand | ResetRequest | Error | Meaning |
| --- | --- | --- | --- | --- | --- | --- |
| `sIdle` | `0000` | FALSE | FALSE | FALSE | FALSE | Block not active |
| `sInit` | `8001` | FALSE | FALSE | FALSE | FALSE | Activated, `S_StartReset` is checked |
| `sWaitEStopIn1` | `8802` | FALSE | TRUE | FALSE | FALSE | First activation, waiting for `S_EStopIn` |
| `sResetError1` | `C001` | FALSE | FALSE | FALSE | TRUE | `Reset` stuck `TRUE` while waiting for `S_EStopIn` |
| `sWaitReset1` | `8402` | FALSE | FALSE | TRUE | FALSE | `S_EStopIn` is `TRUE`, waiting for a rising edge on `Reset` |
| `sSafetyOutputEnabled` | `8000` | TRUE | FALSE | FALSE | FALSE | Safety output enabled |
| `sWaitEStopIn2` | `8804` | FALSE | TRUE | `NOT Reset` | FALSE | Demand detected, waiting for `S_EStopIn` |
| `sResetError2` | `C011` | FALSE | FALSE | FALSE | TRUE | `Reset` stuck `TRUE` while waiting for `S_EStopIn` |
| `sWaitReset2` | `8404` | FALSE | FALSE | `NOT Reset` | FALSE | Waiting for `S_AutoReset` or a rising edge on `Reset` |

Sequence:

1. `REQ` with `Activate = TRUE` in `sIdle` leads to `sInit`. If `S_EStopIn AND S_StartReset`, it goes directly to `sSafetyOutputEnabled`, otherwise to `sWaitEStopIn1`. `S_StartReset` only counts on this very first activation.
2. First activation: in `sWaitEStopIn1`, `Reset = TRUE` leads to the error `sResetError1`; once `S_EStopIn` is `TRUE` again, `sSafetyOutputEnabled` or `sWaitReset1` follows depending on `S_StartReset`. In `sWaitReset1`, the event `EReset` (with `S_EStopIn`) leads to `sSafetyOutputEnabled`; if `S_EStopIn` drops, it goes back to `sWaitEStopIn1`.
3. Operation: in `sSafetyOutputEnabled`, `S_EStopIn = FALSE` leads to `sWaitEStopIn2`.
4. Subsequent cycles: in `sWaitEStopIn2`, `Reset = TRUE` leads to the error `sResetError2`; once `S_EStopIn` is `TRUE` again, `sSafetyOutputEnabled` or `sWaitReset2` follows depending on `S_AutoReset`. In `sWaitReset2`, `S_AutoReset` or the event `EReset` leads to `sSafetyOutputEnabled`; if `S_EStopIn` drops, it goes back to `sWaitEStopIn2`.
5. From any state, `REQ` with `Activate = FALSE` leads back to `sIdle`.

## Technical Details

- The error states `sResetError1`/`sResetError2` prevent a permanently present `Reset` from replacing the acknowledgment (`Reset` must be an edge).
- `ResetRequest` is combined with `NOT Reset` in the subsequent cycles (`sWaitEStopIn2`, `sWaitReset2`).
- The diagnostic codes come from `logiBUS::safety::PLCopen::const::DiagCodes` and `EmergencyStopDiagCodes`.

## Application Scenarios

- Monitoring an emergency stop button with manual acknowledgment via `SF_ResetButton`
- Automatic restart on demand via `S_StartReset`/`S_AutoReset`

## Summary

`SF_EmergencyStop` implements the PLCopen Safety state machine for emergency stop monitoring and enables the safety output only after the sensor has recovered and has been acknowledged.

# logiBUS_PI_IDA_OPC

![logiBUS_PI_IDA_OPC_network](./logiBUS_PI_IDA_OPC_network.svg)

* * * * * * * * * *

## Introduction

`logiBUS_PI_IDA_OPC` connects a physical pulse/counter input (`logiBUS_PI_IDA`, one channel) to two independent VT number fields (raw counter value and the frequency in Hz derived from it) and to OPC-UA: the raw counter value (DWORD) is published directly, the frequency is computed by time derivative (`FT_DERIV_10_AR`) from the counter value, smoothed with a PT1 element (`FT_PT1_AR`) and published separately (REAL, physically scaled via `NumericObjectPool_S`). In addition, the channel can be switched on and off (enable) via a VT key or an OPC-UA switch; a status display (background colour) and an OPC-UA status report disabled/active/fault.

![logiBUS_PI_IDA_OPC](logiBUS_PI_IDA_OPC.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request

### **Event Outputs**

- **INITO** (`EInit`): initialization confirmation (from `Button_SWITCH.INITO`)

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): Quality Input - TRUE enables the services
- **u16ObjId_COUNTVAR** (`UINT`, default `ID_NULL`): object ID of the `CNumberVariable` for the raw counter value (ever-increasing, scale=1 in the VT pool)
- **stObjFreq** (`NumericObjectPool_S`): object ID + scale/offset/decimals of the `CNumberVariable` for the frequency in Hz (own, independent variable, no rescaling of the counter)
- **u16ObjId_SWITCH** (`UINT`, default `ID_NULL`): object ID of the channel on/off key on the VT (a key press toggles the enable, via `Button_IE`)
- **u16ObjId_STATUS** (`UINT`, default `ID_NULL`): object ID of the status display (background colour white = channel disabled, green = active and `logiBUS_PI_IDA.QO` TRUE, red = active and `QO` FALSE, fault)
- **bDefaultEnabled** (`BOOL`): channel state at start (TRUE = active, e.g. channels 1-4; FALSE = inactive, e.g. channels 5-8, since only 4 channels are available at the same time)
- **Input** (`logiBUS::io::PI::logiBUS_PI_S`): selection of the input `PulseInput_I1`..`I8` (default `logiBUS_PI::Invalid`)
- **ID_COUNT_WRITE** (`WSTRING`): counter feedback address (DWORD, raw, `ACTION=PUBLISH`)
- **ID_FREQ_WRITE** (`WSTRING`): frequency feedback address (REAL Hz, `ACTION=PUBLISH`)
- **ID_SWITCH_READ** (`WSTRING`): remote switch address (BOOL, `ACTION=SUBSCRIBE`, toggles the channel enable like a key press)
- **ID_SWITCH_WRITE** (`WSTRING`): echo feedback address (BOOL, `ACTION=PUBLISH`, current channel enable state)
- **ID_STATUS_WRITE** (`WSTRING`): status feedback address (BOOL, `ACTION=PUBLISH`, read-only, `logiBUS_PI_IDA.QO`)

### **Data Outputs**

- **QO** (`BOOL`): Quality Output - TRUE if the services were executed successfully (`AND_QO`, `AND_BOOL_10`: AND of `Q_NumericValue_COUNT.QO`, `Q_NumericValue_FREQ.QO`, `Q_BackgroundColour_EnableOk_STATUS.QO`, `AD_PUBLISH_COUNT.QO`, `AR_PUBLISH_FREQ.QO`, `E_T_FF_SR_SWITCH.QO`, `Button_SWITCH.QO`, `AX_SUBSCRIBE_SWITCH.QO`, `AX_PUBLISH_SWITCH.QO` and `AX_PUBLISH_STATUS.QO`)

## Function Blocks Used

### Sub-blocks: logiBUS_PI_IDA_OPC

- **Type**: SubAppType
- **Internal FBs used**:
    - **logiBUS_PI_IDA**: `logiBUS::io::PI::logiBUS_PI_IDA` — physical pulse input, `ImpulseDelta=DWORD#100`, `TimeDelta=DWORD#250`, `TimeRateLimit=DWORD#100`; its `QI` comes from the channel enable (`E_T_FF_SR_SWITCH.Q`).
    - **AD_SPLIT_2_UNGATED_FREQ**: `adapter::events::unidirectional::AD_SPLIT_2_UNGATED` — branches the DWORD counter value into the counter branch and the frequency branch.
    - **AD_SPLIT_2**: `adapter::events::unidirectional::AD_SPLIT_2` — branches the counter value to the OPC-UA publish and the VT display.
    - **AD_PUBLISH_COUNT**: `adapter::net::AD_PUBLISH_1` — publishes the raw counter value via OPC-UA, target address `ID_COUNT_WRITE`.
    - **AD_TO_AUDI**: `adapter::conversion::unidirectional::AD_TO_AUDI` — bit reinterpretation DWORD→UDINT for the VT counter field.
    - **Q_NumericValue_COUNT**: `isobus::UT::Q::Q_NumericValue_AUDI` — writes the raw counter value to `u16ObjId_COUNTVAR`.
    - **AD_TO_AR_NUM**: `adapter::conversion::unidirectional::AD_TO_AR_NUM` — numerically correct conversion DWORD counter value → REAL for the derivative.
    - **FT_DERIV_10_AR**: `OSCAT_adapter::Control::FT_DERIV_10_AR` (`Faktor=1.0`, `run=TRUE`) — time derivative, delivers the rate of change of the counter value = frequency in Hz; the parameter `K` is an AUI adapter socket fed by `LIT_DerivK` (`adapter::types::unidirectional::AUI::initval::initval_AUI`, `INIT_VAL=9`).
    - **FT_PT1_AR**: `OSCAT_adapter::Control::FT_PT1_AR` (`K=1.0`) — PT1 smoothing of the frequency; the time constant `TM` comes from `initval_ATM` (`adapter::types::unidirectional::ATM::initval::initval_ATM`, `INIT_VAL=T#2s`).
    - **AR_SPLIT_2**: `adapter::events::unidirectional::AR_SPLIT_2` — branches the smoothed frequency value to two outputs.
    - **AR_PUBLISH_FREQ**: `adapter::net::AR_PUBLISH_1` — publishes the frequency via OPC-UA, target address `ID_FREQ_WRITE`.
    - **Q_NumericValue_FREQ**: `isobus::UT::Q::Q_NumericValue_PHYSA` — writes the frequency, physically scaled (`stObjFreq`: `r32Scale`, `i32Offset`, `u8Decimals`), into the VT number field/bargraph.
    - **Button_SWITCH**: `isobus::UT::io::Button::Button_IE` (`InputEvent=BT_PRESSED_LATCHED`) — reads the channel on/off key (`u16ObjId_SWITCH`).
    - **AX_SUBSCRIBE_SWITCH** / **AX_RF_TRIG_SWITCH**: `adapter::net::AX_SUBSCRIBE_1` and `adapter::events::unidirectional::AX_RF_TRIG` — remote switch (`ID_SWITCH_READ`) with edge detection (`ER` → set, `EF` → reset of the enable flip-flop).
    - **E_T_FF_SR_SWITCH**: `iec61499::events::E_T_FF_SR_SYM_INIT` — enable flip-flop (`Q_INIT = bDefaultEnabled`); `Button_SWITCH.IND` clocks `CLK`.
    - **AX_BOOL_TO_X_SWITCH** / **AX_PUBLISH_SWITCH**: `adapter::conversion::unidirectional::AX_BOOL_TO_X` and `adapter::net::AX_PUBLISH_1` — publish the enable state (`ID_SWITCH_WRITE`).
    - **Q_BackgroundColour_EnableOk_STATUS**: SubApp `MyLib::sys::Q_BackgroundColour_EnableOk_A` — status background colour (`u16ObjId_STATUS`, `bEnable` = enable, `bOk` = `logiBUS_PI_IDA.QO`).
    - **AX_BOOL_TO_X_STATUS** / **AX_PUBLISH_STATUS**: `adapter::conversion::unidirectional::AX_BOOL_TO_X` and `adapter::net::AX_PUBLISH_1` — publish `logiBUS_PI_IDA.QO` (`ID_STATUS_WRITE`).
    - **E_MERGE_QO**: `iec61499::events::E_MERGE_3` — merges `Q_NumericValue_FREQ.INITO`, `Q_NumericValue_COUNT.CNF` and `Q_NumericValue_FREQ.CNF`; its output `EO` triggers `AND_QO.REQ`, so `QO` is re-evaluated on every new `Q_NumericValue_COUNT`/`Q_NumericValue_FREQ` value (`CNF`) (up to v1.1 `EO` was not connected and `QO` was only computed at start-up).
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_10` — forms `QO` (see Data Outputs).
- **Functionality**: The raw counter value is branched: one branch goes directly to the OPC-UA publication, a second one via `AD_TO_AUDI` to the VT counter field, a third one via `AD_TO_AR_NUM`, `FT_DERIV_10_AR` and `FT_PT1_AR` to the frequency calculation, whose result is duplicated again (OPC-UA publication and VT display). The channel enable (key, remote switch) releases `logiBUS_PI_IDA`.

## Program Flow and Connections

1. `Input` → `logiBUS_PI_IDA.Input`; `u16ObjId_COUNTVAR` → `Q_NumericValue_COUNT.u16ObjId`; `stObjFreq` → `Q_NumericValue_FREQ.stObj`; `ID_COUNT_WRITE` → `AD_PUBLISH_COUNT.ID`; `ID_FREQ_WRITE` → `AR_PUBLISH_FREQ.ID`.
2. `logiBUS_PI_IDA.IN` (adapter) → `AD_SPLIT_2_UNGATED_FREQ.IN`; `AD_SPLIT_2_UNGATED_FREQ.OUT1` → `AD_SPLIT_2.IN`; `AD_SPLIT_2_UNGATED_FREQ.OUT2` → `AD_TO_AR_NUM.AD_IN`.
3. `AD_SPLIT_2.OUT1` → `AD_PUBLISH_COUNT.IN` (counter OPC-UA branch).
4. `AD_SPLIT_2.OUT2` → `AD_TO_AUDI.AD_IN`; `AD_TO_AUDI.AUDI_OUT` → `Q_NumericValue_COUNT.u32NewValue` (VT counter branch).
5. `AD_TO_AR_NUM.AR_OUT` → `FT_DERIV_10_AR.AR_IN`; `LIT_DerivK.OUT` → `FT_DERIV_10_AR.K`; `FT_DERIV_10_AR.AR_OUT` → `FT_PT1_AR.AR_IN` (with `initval_ATM.OUT` → `FT_PT1_AR.TM`); `FT_PT1_AR.AR_OUT` → `AR_SPLIT_2.IN` (frequency calculation branch).
6. `AR_SPLIT_2.OUT1` → `AR_PUBLISH_FREQ.IN` (frequency OPC-UA branch).
7. `AR_SPLIT_2.OUT2` → `Q_NumericValue_FREQ.rPhys` (frequency VT branch).
8. **Channel enable**: `Button_SWITCH.IND` → `E_T_FF_SR_SWITCH.CLK`; `AX_SUBSCRIBE_SWITCH.OUT` → `AX_RF_TRIG_SWITCH.QI`, `AX_RF_TRIG_SWITCH.ER` → `E_T_FF_SR_SWITCH.S`, `AX_RF_TRIG_SWITCH.EF` → `E_T_FF_SR_SWITCH.R`; `bDefaultEnabled` → `E_T_FF_SR_SWITCH.Q_INIT`; `E_T_FF_SR_SWITCH.Q` → `logiBUS_PI_IDA.QI`, `Q_BackgroundColour_EnableOk_STATUS.bEnable` and `AX_BOOL_TO_X_SWITCH.OUT` (→ `AX_PUBLISH_SWITCH.IN`); `E_T_FF_SR_SWITCH.EO` → `logiBUS_PI_IDA.INIT`, `Q_BackgroundColour_EnableOk_STATUS.REQ` and `AX_BOOL_TO_X_SWITCH.REQ`.
9. **Status**: `logiBUS_PI_IDA.QO` → `Q_BackgroundColour_EnableOk_STATUS.bOk` and `AX_BOOL_TO_X_STATUS.OUT` (→ `AX_PUBLISH_STATUS.IN`); `logiBUS_PI_IDA.INITO` → `Q_BackgroundColour_EnableOk_STATUS.REQ` and `AX_BOOL_TO_X_STATUS.REQ`; `u16ObjId_STATUS` → `Q_BackgroundColour_EnableOk_STATUS.u16ObjId`.
10. **Initialization chain**: `INIT` → `Q_NumericValue_COUNT.INIT` → `Q_NumericValue_FREQ.INIT` → `Q_BackgroundColour_EnableOk_STATUS.INIT` → `AD_PUBLISH_COUNT.INIT` → `Button_SWITCH.INIT` → `INITO` (and → `AND_QO.REQ`; additionally `E_MERGE_QO.EO` → `AND_QO.REQ` triggers it). In parallel, from `AD_PUBLISH_COUNT.INITO` the chain `AR_PUBLISH_FREQ` → `AX_SUBSCRIBE_SWITCH` → `AX_PUBLISH_SWITCH` → `AX_PUBLISH_STATUS` → `E_T_FF_SR_SWITCH` runs (each `INITO` → next `INIT`); `initval_ATM.INITO` → `FT_PT1_AR.INIT`.
11. **Quality**: `QI` → `.QI` of `Q_NumericValue_COUNT`, `Q_NumericValue_FREQ`, `AD_PUBLISH_COUNT`, `AR_PUBLISH_FREQ`, `E_T_FF_SR_SWITCH`, `Button_SWITCH`, `AX_SUBSCRIBE_SWITCH`, `AX_PUBLISH_SWITCH`, `AX_PUBLISH_STATUS` and `Q_BackgroundColour_EnableOk_STATUS`; their `QO` outputs → `AND_QO.IN1`..`IN10`; `AND_QO.OUT` → `QO`.

## Technical Details

- The frequency is not delivered by the hardware FB itself but computed purely from the time derivative (`FT_DERIV_10_AR`, `Faktor=1.0`, `K=9`) of the raw, monotonically increasing counter value and smoothed with `FT_PT1_AR` (`TM=T#2s`) — the counter itself is never reset.
- `K` of `FT_DERIV_10_AR` is an AUI adapter socket and no longer a parameter (version 1.1); the value 9 comes via `LIT_DerivK`.
- `AD_TO_AR_NUM` is deliberately used instead of `AD_TO_AR` to obtain a numerically correct DWORD→REAL conversion rather than a bit reinterpretation (the same pitfall described for [`F_AI_RAW_TO_PERCENT_AD`](./F_AI_RAW_TO_PERCENT_AD.md)).
- Counter value and frequency use independent VT numeric fields (`u16ObjId_COUNTVAR` and `stObjFreq`) with different scaling — the frequency display additionally uses `NumericObjectPool_S` for physical scaling/decimals, the counter does not.
- The `QI` inputs of the internal blocks are no longer hard-wired to `TRUE` but are enabled via the `QI` of the SubApp (exception: `logiBUS_PI_IDA.QI` is tied to the channel enable).

## Application Scenarios

- A flow meter or speed sensor where both the raw (unbounded, ever-increasing) counter value for diagnostic purposes and a derived, immediately interpretable frequency (e.g. Hz, RPM) should be displayed and transmitted; the channel can be switched on and off via a VT key or OPC-UA.

## Summary

Complete VT and OPC-UA connection of a physical pulse input with a parallel raw counter value, a frequency computed from it by time derivative and PT1 smoothing, and a switchable channel enable with status display; `INIT`/`INITO` are chained through the services, `QO` is the AND of the services.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

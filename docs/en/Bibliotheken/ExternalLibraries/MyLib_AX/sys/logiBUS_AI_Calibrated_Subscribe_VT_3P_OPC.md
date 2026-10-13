# logiBUS_AI_Calibrated_Subscribe_VT_3P_OPC

![logiBUS_AI_Calibrated_Subscribe_VT_3P_OPC_network](./logiBUS_AI_Calibrated_Subscribe_VT_3P_OPC_network.svg)

* * * * * * * * * *

## Introduction

`logiBUS_AI_Calibrated_Subscribe_VT_3P_OPC` is the **3-point counterpart** of [`logiBUS_AI_Calibrated_Subscribe_VT_OPC`](./logiBUS_AI_Calibrated_Subscribe_VT_OPC.md) for sensors with a centre position (e.g. slope compensation tilt angle B14). Via remote subscribe (M2M, from the sensor station) it subscribes to an **already calibrated physical value**, shows it on the VT and re-publishes it locally.

The physical VT elements stay where the VT is: the keys **C_MIN**/**C_MID**/**C_MAX** call the methods `ID_MIN_METHOD`/`ID_MID_TRIGGER_METHOD`/`ID_MAX_METHOD` directly on the sensor station via `CLIENT_0` (`logiBUS_AI_Calibrate_3P_IDA_OPC`) - a true event-to-event method call, no toggle/publish/subscribe detour (see `OPC_UA_VERNETZUNG.md`, item 14). The reference value input fields `MIN_REF`/`MID_REF`/`MAX_REF` deliberately stay outside this SubApp (separate `NumericValue_TO_CLIENT_1_0_OPC` instances per caller).

![logiBUS_AI_Calibrated_Subscribe_VT_3P_OPC](logiBUS_AI_Calibrated_Subscribe_VT_3P_OPC.svg)

## Interface Structure

### **Event Inputs**

*No direct event inputs*

### **Event Outputs**

*No direct event outputs*

### **Data Inputs**

Calibrated value:

- **ID_CAL_M2M_READ** (`WSTRING`): Remote subscribe address of the already calibrated physical value (`REAL`); counterpart of `ID_CAL_WRITE` on the sensor station (`logiBUS_AI_Calibrate_3P_IDA_OPC`)
- **stObjCal** (`NumericObjectPool_S`, default `u16ObjId := ID_NULL, r32Scale := 1.0, i32Offset := 0, u8Decimals := 1`): Object ID + scale/offset/decimals of the `CNumberVariable` (calibrated value)
- **ID_WRITE** (`WSTRING`): Local re-publish address (`REAL`, `ACTION=PUBLISH`) for further local consumers on this station

Keys C_MIN/C_MID/C_MAX:

- **u16ObjId_C_MIN**, **u16ObjId_C_MID**, **u16ObjId_C_MAX** (`UINT`, default `ID_NULL`): Object IDs of the keys "C_MIN", "C_MID", "C_MAX", physically here on the VT
- **ID_MIN_CALL**, **ID_MID_TRIGGER_CALL**, **ID_MAX_CALL** (`WSTRING`): Remote method addresses (`ACTION=CALL_METHOD`); call `ID_MIN_METHOD`, `ID_MID_TRIGGER_METHOD` and `ID_MAX_METHOD` respectively directly on the sensor station

Raw value displays (pure diagnostics):

- **ID_RAW_M2M_READ** (`WSTRING`): Remote subscribe address of the uncalibrated raw value (`DWORD` 0-4095); counterpart of `ID_RAW_WRITE` on the sensor station
- **stObjRaw** (`NumericObjectPool_S`, `u8Decimals := 0`): Raw value display field
- **ID_RAW_WRITE** (`WSTRING`): Local re-publish address (`DWORD`, `ACTION=PUBLISH`)
- **ID_MINRAW_M2M_READ** (`WSTRING`): Remote subscribe address of the raw value at the MIN calibration point; counterpart of `ID_MIN_RAW_WRITE` on the sensor station
- **stObjMinRaw** (`NumericObjectPool_S`, `u8Decimals := 0`): Display field for the raw value at the MIN calibration point
- **ID_MINRAW_WRITE** (`WSTRING`): Local re-publish address (`DWORD`, `ACTION=PUBLISH`)
- **ID_MIDRAW_M2M_READ** (`WSTRING`): Remote subscribe address of the raw value at the MID calibration point; counterpart of `ID_MID_RAW_WRITE` on the sensor station
- **stObjMidRaw** (`NumericObjectPool_S`, `u8Decimals := 0`): Display field for the raw value at the MID calibration point
- **ID_MIDRAW_WRITE** (`WSTRING`): Local re-publish address (`DWORD`, `ACTION=PUBLISH`)
- **ID_MAXRAW_M2M_READ** (`WSTRING`): Remote subscribe address of the raw value at the MAX calibration point; counterpart of `ID_MAX_RAW_WRITE` on the sensor station
- **stObjMaxRaw** (`NumericObjectPool_S`, `u8Decimals := 0`): Display field for the raw value at the MAX calibration point
- **ID_MAXRAW_WRITE** (`WSTRING`): Local re-publish address (`DWORD`, `ACTION=PUBLISH`)

### **Data Outputs**

*No data outputs*

### **Adapters**

- **CAL_OUT** (plug, `adapter::types::unidirectional::AR`): Read-only mirror of the calibrated value (`ValueDisplay.VALUE_OUT`) for further local consumers on this station (e.g. [`Fahrwerk_Mittelpunkt_Anzeige`](./Fahrwerk_Mittelpunkt_Anzeige.md) for bar graph/status centre), instead of its own OPC-UA self-subscribe

## Function Blocks (FBs) Used

- **ValueDisplay**: SubApp [`MyLib::sys::AR_M2M_Subscribe_TO_Q_Publish_OPC`](./AR_M2M_Subscribe_TO_Q_Publish_OPC.md) - calibrated value
- **RawDisplay**, **MinRawDisplay**, **MidRawDisplay**, **MaxRawDisplay**: the same SubApp for the raw value and the raw values of the three calibration points
- **TriggerMIN**, **TriggerMID**, **TriggerMAX**: SubApp [`MyLib::sys::Button_CLIENT_Method_IE`](./Button_CLIENT_Method_IE.md) (`InputEvent=BT_PRESSED_LATCHED`) - key → remote method call (`ID_CALL`)
- **Imports**: `ID_NULL`, `BT_PRESSED_LATCHED`, `NumericObjectPool_S`

## Program Flow and Connections

- `ID_CAL_M2M_READ`, `stObjCal`, `ID_WRITE` → `ValueDisplay` (`ID_M2M_READ`, `stObj`, `ID_WRITE`); `ValueDisplay.VALUE_OUT` → `CAL_OUT`
- `u16ObjId_C_MIN`/`ID_MIN_CALL` → `TriggerMIN` (`u16ObjId`/`ID_CALL`); likewise `C_MID` → `TriggerMID`, `C_MAX` → `TriggerMAX`
- `ID_RAW_M2M_READ`, `stObjRaw`, `ID_RAW_WRITE` → `RawDisplay`
- `ID_MINRAW_M2M_READ`, `stObjMinRaw`, `ID_MINRAW_WRITE` → `MinRawDisplay`
- `ID_MIDRAW_M2M_READ`, `stObjMidRaw`, `ID_MIDRAW_WRITE` → `MidRawDisplay`
- `ID_MAXRAW_M2M_READ`, `stObjMaxRaw`, `ID_MAXRAW_WRITE` → `MaxRawDisplay`

## Technical Details

- The calibration itself runs on the sensor station; this block contains no calibration logic
- Version 1.1 (2026-09-26): plug `CAL_OUT` added - mirrors the already remotely subscribed value for further consumers on STG1, no additional self-subscribe needed
- Version 1.0 (2026-09-26): 3-point counterpart of `logiBUS_AI_Calibrated_Subscribe_VT_OPC` v1.8, for slope compensation angle B14 (calibration moves from STG1 to STG3)

## Application Scenarios

- VT station that displays the calibrated tilt angle (B14) of another station and triggers the 3-point calibration (MIN/MID/MAX) via keys

## Summary

`logiBUS_AI_Calibrated_Subscribe_VT_3P_OPC` shows remotely calibrated values and raw values of the calibration points on the VT and forwards the keys C_MIN/C_MID/C_MAX to the sensor station via OPC-UA method calls.

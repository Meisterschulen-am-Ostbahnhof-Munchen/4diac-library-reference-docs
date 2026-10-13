# logiBUS_AI_Calibrate_3P_IDA_OPC

![logiBUS_AI_Calibrate_3P_IDA_OPC_network](./logiBUS_AI_Calibrate_3P_IDA_OPC_network.svg)

* * * * * * * * * *

## Introduction

`logiBUS_AI_Calibrate_3P_IDA_OPC` reads a logiBUS analog input (`logiBUS_AI_IDA`) and calibrates it with a **3-point calibration** (`AR_CALIBRATE_3P_REF`): the three raw values `X_MIN`/`X_MID`/`X_MAX` and the three reference values `MIN_REF`/`MID_REF`/`MAX_REF` are stored independently of each other and all persisted in an INI file. The raw and the calibrated value are output to VT number fields (each its own `CNumberVariable`), three VT keys (`C_MIN`/`C_MID`/`C_MAX`) trigger the calibration, and the values are published via OPC-UA (raw value `DWORD`, calibrated value `REAL`; generic, one channel). The block is intended for sensors with a defined center position (e.g. steering angle sensor: left/straight/right) instead of a pure 2-point linearization.

![logiBUS_AI_Calibrate_3P_IDA_OPC](logiBUS_AI_Calibrate_3P_IDA_OPC.svg)

## Interface Structure

### **Event Inputs**

*No direct event inputs*

### **Event Outputs**

*No direct event outputs*

### **Data Inputs**

Input and display:

- **Input** (`logiBUS::io::AI::logiBUS_AI_S`, default `logiBUS_AI::Invalid`): Identifies the analog input `AnalogInput_I1..I8`
- **u16ObjId_RAWVAR** (`UINT`, default `ID_NULL`): Object ID of the `CNumberVariable` for the uncalibrated raw value (0-4095, pure diagnostic display)
- **stObjCal** (`NumericObjectPool_S`): Object ID + scale/offset/decimals of the `CNumberVariable` for the calibrated value (own, independent variable)
- **sSection** (`STRING`): INI section name for the persistence of `X_MIN`/`X_MID`/`X_MAX`/`MIN_REF`/`MID_REF`/`MAX_REF`; unique per channel, otherwise the channels collide in `settings.ini`

VT keys and methods:

- **u16ObjId_C_MIN**, **u16ObjId_C_MID**, **u16ObjId_C_MAX** (`UINT`, default `ID_NULL`): Object IDs of the keys `C_MIN` (approach min position, then press), `C_MID` (center position) and `C_MAX` (max position)
- **ID_MIN_METHOD**, **ID_MID_TRIGGER_METHOD**, **ID_MAX_METHOD** (`WSTRING`): Method addresses (`ACTION=CREATE_METHOD`) for the argument-less triggers; equivalent to the local VT key, callable from the OPC-UA dashboard and via `CLIENT_0` from another station

Reference values (VT input field + OPC-UA):

- **stObjMinRef**, **stObjMidRef**, **stObjMaxRef** (`NumericObjectPool_S`): Input fields for `MIN_REF`/`MID_REF`/`MAX_REF`
- **ID_MINREF_READ**, **ID_MIDREF_READ**, **ID_MAXREF_READ** (`WSTRING`): Remote subscribe addresses of the reference values (`REAL`, `ACTION=SUBSCRIBE`)
- **ID_MINREF_WRITE**, **ID_MIDREF_WRITE**, **ID_MAXREF_WRITE** (`WSTRING`): Remote echo addresses of the reference values (`REAL`, `ACTION=PUBLISH`)

Raw values at the calibration points (pure diagnostic display):

- **stObjMinRaw**, **stObjMidRaw**, **stObjMaxRaw** (`NumericObjectPool_S`): Display fields for the raw value at the MIN/MID/MAX calibration point
- **ID_MIN_RAW_WRITE**, **ID_MID_RAW_WRITE**, **ID_MAX_RAW_WRITE** (`WSTRING`): Publish addresses (`REAL`, `ACTION=PUBLISH`)

OPC-UA outputs:

- **ID_RAW_WRITE** (`WSTRING`): Raw value feedback address (`DWORD` 0-4095, `ACTION=PUBLISH`)
- **ID_CAL_WRITE** (`WSTRING`): Feedback address of the calibrated value (`REAL`, `ACTION=PUBLISH`)

### **Data Outputs**

*No data outputs*

### **Adapters**

This function block has no adapters at its interface; `AR` adapters are used internally.

## Function Blocks (FBs) Used

- **InputRaw**: SubApp `MyLib::sys::logiBUS_AI_Raw_VT_AR_OPC` - reads the analog input, displays the raw value and publishes it
- **CALIBRATE**: `adapter::Engineering::measurements::AR_CALIBRATE_3P_REF` (`QI=TRUE`, `xClipping=FALSE`) - 3-point calibration
- **MIN_REF_LIT**, **MID_REF_LIT**, **MAX_REF_LIT**: SubApp `MyLib::sys::NumericValue_TO_AR2_OPC` - reference values from VT input field or OPC-UA
- **TriggerMIN**, **TriggerMID**, **TriggerMAX**: SubApp `MyLib::sys::Button_Method_IE` - key OR local method → `IND`
- **INI_X_MIN**, **INI_X_MID**, **INI_X_MAX**: `eclipse4diac::storage::INI_AR2` (keys `X_MIN`/`X_MID`/`X_MAX`, default `0.0`)
- **INI_MIN_REF**, **INI_MID_REF**, **INI_MAX_REF**: `INI_AR2` (keys `MIN_REF`/`MID_REF`/`MAX_REF`, defaults `-100.0`/`0.0`/`100.0`)
- **TapMinRaw**, **TapMidRaw**, **TapMaxRaw**: `adapter::events::bidirectional::AR2_SPLIT_FWD_REV`
- **PublishCal**, **PublishMinRaw**, **PublishMidRaw**, **PublishMaxRaw**: SubApp `MyLib::sys::AR_TO_Q_Publish_OPC` - VT display and OPC-UA publish

All `INI_AR2` use `QI=TRUE` and `SETM=TRUE`; `sSection` is their common `SECTION`.

## Program Flow and Connections

1. `InputRaw.AR_OUT` → `CALIBRATE.X` (raw value).
2. `TriggerMIN.IND`/`TriggerMID.IND`/`TriggerMAX.IND` → `CALIBRATE.C_MIN`/`C_MID`/`C_MAX`.
3. Reference values: `MIN_REF_LIT.VALUEO`/`MID_REF_LIT.VALUEO`/`MAX_REF_LIT.VALUEO` → `CALIBRATE.Y_MIN`/`Y_MID`/`Y_MAX`.
4. Stored raw values: `CALIBRATE.X_MIN`/`X_MID`/`X_MAX` → `TapMinRaw.IN`/`TapMidRaw.IN`/`TapMaxRaw.IN`; their `OUT` → `INI_X_MIN.VAL`/`INI_X_MID.VAL`/`INI_X_MAX.VAL`; `REV_OUT` → `PublishMinRaw`/`PublishMidRaw`/`PublishMaxRaw.AR_IN`.
5. Reference values (persistence): `CALIBRATE.MIN_REF`/`MID_REF`/`MAX_REF` → `INI_MIN_REF.VAL`/`INI_MID_REF.VAL`/`INI_MAX_REF.VAL`.
6. Calibrated value: `CALIBRATE.Y` → `PublishCal.AR_IN`.

## Technical Details

- All six calibration corner values (three raw values, three reference values) are INI-persisted independently of each other via one `INI_AR2` each.
- `xClipping=FALSE`: the calibrated value is not clamped.
- `sSection` must be unique per channel.

## Application Scenarios

- Sensors with a defined center position, e.g. steering angle sensor (left/straight/right)
- Calibration via VT key, OPC-UA dashboard or remote call from another station

## Summary

`logiBUS_AI_Calibrate_3P_IDA_OPC` bundles analog input, 3-point calibration, INI persistence, VT display and OPC-UA connection of one channel in one SubApp.

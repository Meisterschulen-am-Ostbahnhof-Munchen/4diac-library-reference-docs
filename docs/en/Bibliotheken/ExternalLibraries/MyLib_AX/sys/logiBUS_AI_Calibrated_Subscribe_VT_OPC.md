# logiBUS_AI_Calibrated_Subscribe_VT_OPC

![logiBUS_AI_Calibrated_Subscribe_VT_OPC_network](./logiBUS_AI_Calibrated_Subscribe_VT_OPC_network.svg)

* * * * * * * * * *

## Introduction

`logiBUS_AI_Calibrated_Subscribe_VT_OPC` subscribes via remote subscribe (M2M, from the sensor station) to an **already calibrated physical value** and shows it on the VT (`Q_NumericValue_PHYSA`) with a local re-publish. The physical VT elements stay where the VT is: the keys **CO**/**CS** call the methods `ID_CO_METHOD`/`ID_CS_METHOD` directly on the sensor station (`logiBUS_AI_Calibrate_IDA_OPC`) via `CLIENT_0` - a genuine event-to-event method call, no toggle/publish/subscribe detour (see `OPC_UA_VERNETZUNG.md`, item 14).

The block replaces `logiBUS_AI_Calibrate_Remote_IDA_OPC`: the calibration itself now runs on the sensor station, only the display and the forwarding of the VT controls remain here. The `ZERO`/`SPAN` reference value input fields (`stObjZero`/`stObjSpan`) are optional: leave `ID_NULL` if the sensor has fixed reference values (filling belt angle: 0/100 as a literal on the sensor station), pass real values through if the operator enters them (pressure: 0/250 bar, no calibrator always at hand).

![logiBUS_AI_Calibrated_Subscribe_VT_OPC](logiBUS_AI_Calibrated_Subscribe_VT_OPC.svg)

## Interface Structure

### **Event Inputs**

*No direct event inputs*

### **Event Outputs**

*No direct event outputs*

### **Data Inputs**

Calibrated value:

- **ID_CAL_M2M_READ** (`WSTRING`): Remote subscribe address of the already calibrated physical value (`REAL`); counterpart of `ID_CAL_WRITE` on the sensor station
- **stObjCal** (`NumericObjectPool_S`): Object ID + scale/offset/decimals of the `CNumberVariable` (calibrated value)
- **ID_WRITE** (`WSTRING`): Local re-publish address (`REAL`, `ACTION=PUBLISH`) for further local consumers

Keys CO/CS:

- **u16ObjId_CO** (`UINT`, default `ID_NULL`): Object ID of the key "CO" (calibration zero point), physically here on the VT
- **u16ObjId_CS** (`UINT`, default `ID_NULL`): Object ID of the key "CS" (calibration span), physically here on the VT
- **ID_CO_CALL**, **ID_CS_CALL** (`WSTRING`): Remote method addresses (`ACTION=CALL_METHOD`); call `ID_CO_METHOD`/`ID_CS_METHOD` directly on the sensor station

Raw value displays (pure diagnostics):

- **ID_RAW_M2M_READ** (`WSTRING`): Remote subscribe address of the uncalibrated raw value (`DWORD` 0-4095); counterpart of `ID_RAW_WRITE` on the sensor station
- **stObjRaw** (`NumericObjectPool_S`, `u8Decimals := 0`): Display field raw value
- **ID_RAW_WRITE** (`WSTRING`): Local re-publish address (`DWORD`, `ACTION=PUBLISH`)
- **ID_MINRAW_M2M_READ** (`WSTRING`): Remote subscribe address of the raw value at the MIN calibration point; counterpart of `ID_ZERO_RAW_WRITE` on the sensor station
- **stObjMinRaw** (`NumericObjectPool_S`, `u8Decimals := 0`): Display field raw value at the MIN calibration point
- **ID_MINRAW_WRITE** (`WSTRING`): Local re-publish address (`DWORD`, `ACTION=PUBLISH`)
- **ID_MAXRAW_M2M_READ** (`WSTRING`): Remote subscribe address of the raw value at the MAX calibration point; counterpart of `ID_SPAN_RAW_WRITE` on the sensor station
- **stObjMaxRaw** (`NumericObjectPool_S`, `u8Decimals := 0`): Display field raw value at the MAX calibration point
- **ID_MAXRAW_WRITE** (`WSTRING`): Local re-publish address (`DWORD`, `ACTION=PUBLISH`)

Reference values (optional):

- **stObjZero**, **stObjSpan** (`NumericObjectPool_S`): VT input fields for the ZERO/SPAN reference value; leave `ID_NULL` if the sensor has fixed reference values
- **ID_ZERO_READ**, **ID_SPAN_READ** (`WSTRING`): Remote subscribe addresses (`REAL`, `ACTION=SUBSCRIBE`) - optional web override of the VT input field, last writer wins
- **ID_ZERO_WRITE**, **ID_SPAN_WRITE** (`WSTRING`): Publish addresses (`REAL`, `ACTION=PUBLISH`), subscribed remotely by the sensor station (counterpart of its `ID_ZERO_READ`/`ID_SPAN_READ`)

### **Data Outputs**

*No data outputs*

### **Adapters**

- **CAL_OUT** (plug, `adapter::types::unidirectional::AR`): Read-only mirror of the calibrated value (`ValueDisplay.VALUE_OUT`) for further local consumers on this station, instead of its own OPC-UA self-subscribe

## Function Blocks (FBs) Used

- **ValueDisplay**: SubApp `MyLib::sys::AR_M2M_Subscribe_TO_Q_Publish_OPC` - calibrated value
- **RawDisplay**, **MinRawDisplay**, **MaxRawDisplay**: same SubApp for the raw value and the raw values of the calibration points
- **TriggerCO**, **TriggerCS**: SubApp [`MyLib::sys::Button_CLIENT_Method_IE`](./Button_CLIENT_Method_IE.md) (`InputEvent=BT_PRESSED_LATCHED`)
- **ZeroRef**, **SpanRef**: SubApp `MyLib::sys::NumericValue_TO_AR2_OPC` - reference value input fields

## Program Flow and Connections

- `ID_CAL_M2M_READ`, `stObjCal`, `ID_WRITE` → `ValueDisplay`; `ValueDisplay.VALUE_OUT` → `CAL_OUT`
- `u16ObjId_CO`/`ID_CO_CALL` → `TriggerCO`; `u16ObjId_CS`/`ID_CS_CALL` → `TriggerCS`
- `ID_RAW_M2M_READ`, `stObjRaw`, `ID_RAW_WRITE` → `RawDisplay`
- `ID_MINRAW_M2M_READ`, `stObjMinRaw`, `ID_MINRAW_WRITE` → `MinRawDisplay`
- `ID_MAXRAW_M2M_READ`, `stObjMaxRaw`, `ID_MAXRAW_WRITE` → `MaxRawDisplay`
- `stObjZero`, `ID_ZERO_READ`, `ID_ZERO_WRITE` → `ZeroRef`
- `stObjSpan`, `ID_SPAN_READ`, `ID_SPAN_WRITE` → `SpanRef`

## Technical Details

- The button + `CLIENT_0` call formerly occurred twice identically here (CO and CS) and was factored out into `Button_CLIENT_Method_IE`.
- The calibration itself runs on the sensor station; this block contains no calibration logic.

## Application Scenarios

- VT station that displays calibrated sensor values of another station and triggers its calibration via key (e.g. filling belt angle, pressure)

## Summary

`logiBUS_AI_Calibrated_Subscribe_VT_OPC` shows remotely calibrated values and raw values on the VT and forwards the calibration keys to the sensor station via OPC-UA method call.

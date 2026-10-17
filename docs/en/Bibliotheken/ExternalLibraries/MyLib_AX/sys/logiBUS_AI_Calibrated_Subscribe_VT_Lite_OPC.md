# logiBUS_AI_Calibrated_Subscribe_VT_Lite_OPC

![logiBUS_AI_Calibrated_Subscribe_VT_Lite_OPC_network](./logiBUS_AI_Calibrated_Subscribe_VT_Lite_OPC_network.svg)

* * * * * * * * * *

## Introduction

`logiBUS_AI_Calibrated_Subscribe_VT_Lite_OPC` is the lean variant of [`logiBUS_AI_Calibrated_Subscribe_VT_OPC`](./logiBUS_AI_Calibrated_Subscribe_VT_OPC.md) for sensors with fixed reference values. The calibrated value, the raw value and the raw values at the MIN and MAX calibration points are subscribed remotely from the sensor station and shown on the VT; the keys **CO**/**CS** call the calibration methods on the sensor station via `CALL_METHOD`. Compared with the original there are no local re-publishes (no `ID_*_WRITE` mirrors) and no `ZERO`/`SPAN` reference value input fields (`ZeroRef`/`SpanRef` are omitted, the reference values are fixed on the sensor station). Instead of `AR_M2M_Subscribe_TO_Q_Publish_OPC`, [`AR_M2M_Subscribe_TO_Q_OPC`](./AR_M2M_Subscribe_TO_Q_OPC.md) is used four times.

![logiBUS_AI_Calibrated_Subscribe_VT_Lite_OPC](logiBUS_AI_Calibrated_Subscribe_VT_Lite_OPC.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request; starts the chain `ValueDisplay` → `TriggerCO` → `TriggerCS` → `RawDisplay` → `MinRawDisplay` → `MaxRawDisplay`

### **Event Outputs**

- **INITO** (`EInit`): initialization confirmation; comes from `MaxRawDisplay.INITO`

### **Data Inputs**

| Variable | Type | Default | Comment |
|----------|------|---------|---------|
| `QI` | BOOL | TRUE | Quality input: enables the service; passed on to all six instances. |
| `ID_CAL_M2M_READ` | WSTRING |  | Remote subscribe address of the calibrated value (`REAL`) of the sensor station. |
| `stObjCal` | `logiBUS::utils::conversion::phys::NumericObjectPool_S` | `(u16ObjId := ID_NULL, r32Scale := 1.0, i32Offset := 0, u8Decimals := 1)` | Object ID plus scale/offset/decimals of the `CNumberVariable` (calibrated value). |
| `u16ObjId_CO` | UINT | `ID_NULL` | Object ID of the key "CO" (calibration zero point) on the VT. |
| `u16ObjId_CS` | UINT | `ID_NULL` | Object ID of the key "CS" (calibration span) on the VT. |
| `ID_CO_CALL` | WSTRING |  | Remote method address (`CALL_METHOD`) for the CO trigger on the sensor station. |
| `ID_CS_CALL` | WSTRING |  | Remote method address (`CALL_METHOD`) for the CS trigger on the sensor station. |
| `ID_RAW_M2M_READ` | WSTRING |  | Remote subscribe address of the raw value (diagnostic display). |
| `stObjRaw` | `logiBUS::utils::conversion::phys::NumericObjectPool_S` | `(u16ObjId := ID_NULL, r32Scale := 1.0, i32Offset := 0, u8Decimals := 0)` | Object ID plus scale/offset/decimals of the `CNumberVariable` (raw value). |
| `ID_MINRAW_M2M_READ` | WSTRING |  | Remote subscribe address of the raw value at the MIN calibration point (diagnostic display). |
| `stObjMinRaw` | `logiBUS::utils::conversion::phys::NumericObjectPool_S` | `(u16ObjId := ID_NULL, r32Scale := 1.0, i32Offset := 0, u8Decimals := 0)` | Object ID plus scale/offset/decimals of the `CNumberVariable` (raw value MIN). |
| `ID_MAXRAW_M2M_READ` | WSTRING |  | Remote subscribe address of the raw value at the MAX calibration point (diagnostic display). |
| `stObjMaxRaw` | `logiBUS::utils::conversion::phys::NumericObjectPool_S` | `(u16ObjId := ID_NULL, r32Scale := 1.0, i32Offset := 0, u8Decimals := 0)` | Object ID plus scale/offset/decimals of the `CNumberVariable` (raw value MAX). |

### **Data Outputs**

| Variable | Type | Default | Comment |
|----------|------|---------|---------|
| `QO` | BOOL |  | Quality output: `AND` of the `QO` outputs of all six instances (via `AND_QO`, `AND_BOOL_6`). |

### **Adapters**

| Adapter | Type | Direction | Comment |
|---------|------|-----------|---------|
| `CAL_OUT` | `adapter::types::unidirectional::AR` | Plug | Read-only mirror of the calibrated value (`ValueDisplay.VALUE_OUT`) for further local consumers. |

## Function Blocks (FBs) Used

- **ValueDisplay** (SubApp `MyLib::sys::AR_M2M_Subscribe_TO_Q_OPC`): calibrated value
- **TriggerCO**, **TriggerCS** (SubApp [`MyLib::sys::Button_CLIENT_Method_IE`](./Button_CLIENT_Method_IE.md), `InputEvent = BT_PRESSED_LATCHED`): keys CO and CS with method call
- **RawDisplay**, **MinRawDisplay**, **MaxRawDisplay** (SubApp `MyLib::sys::AR_M2M_Subscribe_TO_Q_OPC`): raw value and raw values of the MIN and MAX calibration points
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_6`): combines the six `QO` outputs

## Program Flow and Connections

1. Value: `ID_CAL_M2M_READ` -> `ValueDisplay.ID_M2M_READ`; `stObjCal` -> `ValueDisplay.stObj`; `ValueDisplay.VALUE_OUT` -> `CAL_OUT`.
2. Keys: `u16ObjId_CO` -> `TriggerCO.u16ObjId`; `ID_CO_CALL` -> `TriggerCO.ID_CALL`; `u16ObjId_CS` -> `TriggerCS.u16ObjId`; `ID_CS_CALL` -> `TriggerCS.ID_CALL`.
3. Raw value: `ID_RAW_M2M_READ` -> `RawDisplay.ID_M2M_READ`; `stObjRaw` -> `RawDisplay.stObj`.
4. MIN raw value: `ID_MINRAW_M2M_READ` -> `MinRawDisplay.ID_M2M_READ`; `stObjMinRaw` -> `MinRawDisplay.stObj`.
5. MAX raw value: `ID_MAXRAW_M2M_READ` -> `MaxRawDisplay.ID_M2M_READ`; `stObjMaxRaw` -> `MaxRawDisplay.stObj`.
6. Initialization: `INIT` -> `ValueDisplay.INIT` -> `TriggerCO.INIT` -> `TriggerCS.INIT` -> `RawDisplay.INIT` -> `MinRawDisplay.INIT` -> `MaxRawDisplay.INIT` (each via `INITO`); `MaxRawDisplay.INITO` -> `INITO` and -> `AND_QO.REQ`.
7. Qualifier: `QI` -> `.QI` of the six instances.
8. Quality: `ValueDisplay.QO` -> `AND_QO.IN1`; `TriggerCO.QO` -> `IN2`; `TriggerCS.QO` -> `IN3`; `RawDisplay.QO` -> `IN4`; `MinRawDisplay.QO` -> `IN5`; `MaxRawDisplay.QO` -> `IN6`; `AND_QO.OUT` -> `QO`.

## Technical Features

- Without local re-publishes: there are no `ID_*_WRITE` inputs.
- Without `ZERO`/`SPAN`: the reference values are fixed on the sensor station (e.g. filling belt angle).
- The calibration itself runs on the sensor station; this block contains no calibration logic.

## Application Scenarios

- VT station that shows calibrated sensor values of another station and triggers their calibration by key, when the sensor has fixed reference values (e.g. filling belt angle) and no local re-publishes are needed.

## Summary

`logiBUS_AI_Calibrated_Subscribe_VT_Lite_OPC` shows remotely calibrated values and raw values on the VT and forwards the calibration keys to the sensor station via method call - without local re-publishes and without `ZERO`/`SPAN`.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

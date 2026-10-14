# logiBUS_AI_Calibrate_3P_Subscribe_VT_OPC

![logiBUS_AI_Calibrate_3P_Subscribe_VT_OPC_network](./logiBUS_AI_Calibrate_3P_Subscribe_VT_OPC_network.svg)

![logiBUS_AI_Calibrate_3P_Subscribe_VT_OPC](./logiBUS_AI_Calibrate_3P_Subscribe_VT_OPC.svg)

* * * * * * * * * *

## Introduction

`logiBUS_AI_Calibrate_3P_Subscribe_VT_OPC` is the VT-side counterpart to the sensor station [`logiBUS_AI_Calibrate_3P_IDA_OPC`](./logiBUS_AI_Calibrate_3P_IDA_OPC.md): the SubApp subscribes via OPC-UA (M2M) to the **already calibrated** physical value of the 3-point calibration, shows it in the VT, re-publishes it locally and optionally provides it on the plug `CAL_OUT` for local consumers. The physical VT elements stay here (where the VT is): the keys `C_MIN`/`C_MID`/`C_MAX` call the corresponding methods on the sensor station directly via `CLIENT_0` (a genuine event-to-event method call, no toggle/publish/subscribe detour). The three reference value input fields `MIN_REF`/`MID_REF`/`MAX_REF` (`NumericValue_TO_AR2_OPC`) are still published **locally** and subscribed remotely by the sensor station; `MID_REF` and `MAX_REF` are additionally re-exposed as plain `AR` plugs (`MIDREF_OUT`/`MAXREF_OUT`). Used by the 3-point calibration training sample [InputOutputTesterButton_AI_Calibrate_3P_OPC_UA](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Meins/InputOutputTester/Button_AI_Calibrate_3P_OPC_UA/InputOutputTesterButton_AI_Calibrate_3P_OPC_UA/).

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request

### **Event Outputs**

- **INITO** (`EInit`): initialization confirmation (from `MaxRef.INITO`)

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): Quality Input - TRUE enables the services
- **ID_CAL_M2M_READ** (`WSTRING`): remote subscribe address of the already calibrated physical value (`REAL`), counterpart of `ID_CAL_WRITE` on the sensor station
- **stObjCal** (`NumericObjectPool_S`): object ID + scale/offset/decimals of the `CNumberVariable` for the calibrated value
- **ID_WRITE** (`WSTRING`): local re-publish address (`REAL`, `ACTION=PUBLISH`) for further local consumers
- **u16ObjId_C_MIN**, **u16ObjId_C_MID**, **u16ObjId_C_MAX** (`UINT`, default `ID_NULL`): object IDs of the keys `C_MIN`/`C_MID`/`C_MAX` on the VT
- **ID_MIN_CALL**, **ID_MID_CALL**, **ID_MAX_CALL** (`WSTRING`): remote method addresses (`ACTION=CALL_METHOD`) for the three calibration triggers on the sensor station
- **stObjMinRef**, **stObjMidRef**, **stObjMaxRef** (`NumericObjectPool_S`): object ID + scale/offset/decimals of the reference value input fields on this station
- **ID_MINREF_READ**, **ID_MIDREF_READ**, **ID_MAXREF_READ** (`WSTRING`): optional subscribe addresses if the reference value is to be overridden externally (dashboard); may be left unused
- **ID_MINREF_WRITE**, **ID_MIDREF_WRITE**, **ID_MAXREF_WRITE** (`WSTRING`): local publish addresses (`REAL`, `ACTION=PUBLISH`) of the reference values, subscribed remotely by the sensor station

### **Data Outputs**

- **QO** (`BOOL`): Quality Output - TRUE if all services succeeded (AND of `AND_QO_A.OUT` and `ValueDisplay.QO`)

### **Adapters**

- **CAL_OUT** (plug, `adapter::types::unidirectional::AR`): optional, calibrated value (plain `AR` mirror of the remote subscribe) for external local consumers; leave unwired if not needed
- **MIDREF_OUT** (plug, `adapter::types::unidirectional::AR`): optional, live `MID_REF` reference value
- **MAXREF_OUT** (plug, `adapter::types::unidirectional::AR`): optional, live `MAX_REF` reference value

## Function blocks used

- **`AR_SUBSCRIBE_CAL`** (`adapter::net::AR_SUBSCRIBE_1`): subscribes to the calibrated value from the sensor station (`ID_CAL_M2M_READ`).
- **`AR_SPLIT_2_Cal`** (`adapter::events::unidirectional::AR_SPLIT_2`): distributes the value to `ValueDisplay` and `CAL_OUT`.
- **`ValueDisplay`** (SubApp [`AR_TO_Q_Publish_OPC`](./AR_TO_Q_Publish_OPC.md)): VT display and local re-publish (`stObjCal`, `ID_WRITE`).
- **`Button_C_MIN`**, **`Button_C_MID`**, **`Button_C_MAX`** (`isobus::UT::io::Button::Button_IE`, `InputEvent = BT_PRESSED_LATCHED`): read the three VT keys.
- **`CLIENT_MIN`**, **`CLIENT_MID`**, **`CLIENT_MAX`** (`iec61499::net::CLIENT_0`): call the methods on the sensor station (`ID_MIN_CALL`/`ID_MID_CALL`/`ID_MAX_CALL`).
- **`MinRef`**, **`MidRef`**, **`MaxRef`** (SubApp [`NumericValue_TO_AR2_OPC`](./NumericValue_TO_AR2_OPC.md)): VT input fields of the reference values, published locally.
- **`AND_QO_A`** (`iec61131::booleanOperators::AND_BOOL_10`) and **`AND_QO`** (`iec61131::booleanOperators::AND_BOOL_2`): combine the `QO` outputs of all sub-blocks into the `QO` of the SubApp.

## Functionality

- **Calibrated value:** `AR_SUBSCRIBE_CAL.OUT` → `AR_SPLIT_2_Cal.IN`; `AR_SPLIT_2_Cal.OUT1` → `ValueDisplay.AR_IN`; `AR_SPLIT_2_Cal.OUT2` → `CAL_OUT`.
- **Calibration keys:** `Button_C_MIN.IND` → `CLIENT_MIN.REQ`, `Button_C_MID.IND` → `CLIENT_MID.REQ`, `Button_C_MAX.IND` → `CLIENT_MAX.REQ`.
- **Reference values:** `MinRef`/`MidRef`/`MaxRef` receive `stObjMinRef`/`stObjMidRef`/`stObjMaxRef`, `ID_*REF_READ` and `ID_*REF_WRITE`; `MidRef.VALUE_OUT` → `MIDREF_OUT`, `MaxRef.VALUE_OUT` → `MAXREF_OUT`.
- **Initialization chain:** `INIT` → `AR_SUBSCRIBE_CAL` → `ValueDisplay` → `Button_C_MIN` → `Button_C_MID` → `Button_C_MAX` → `CLIENT_MIN` → `CLIENT_MID` → `CLIENT_MAX` → `MinRef` → `MidRef` → `MaxRef` (each `INITO` → next `INIT`); `MaxRef.INITO` → `INITO` and → `AND_QO_A.REQ`; `AND_QO_A.CNF` → `AND_QO.REQ`.
- **Quality:** `QI` → `.QI` of `AR_SUBSCRIBE_CAL`, `Button_C_MIN`/`_MID`/`_MAX`, `CLIENT_MIN`/`_MID`/`_MAX`, `MinRef`, `MidRef`, `MaxRef` and `ValueDisplay`. `AR_SUBSCRIBE_CAL.QO`, the three `Button_C_*.QO`, the three `CLIENT_*.QO` and `MinRef.QO`/`MidRef.QO`/`MaxRef.QO` → `AND_QO_A.IN1`..`IN10`; `AND_QO_A.OUT` → `AND_QO.IN1`, `ValueDisplay.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`.

## Summary

Remote display block for an already calibrated 3-point value: subscribe of the calibrated value, VT display with local re-publish, calibration keys as method calls to the sensor station, and locally published reference value input fields in one composite.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

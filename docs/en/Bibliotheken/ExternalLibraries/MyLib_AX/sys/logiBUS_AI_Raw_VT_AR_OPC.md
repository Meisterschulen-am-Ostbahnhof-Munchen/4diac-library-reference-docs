# logiBUS_AI_Raw_VT_AR_OPC

![logiBUS_AI_Raw_VT_AR_OPC_network](./logiBUS_AI_Raw_VT_AR_OPC_network.svg)

* * * * * * * * * *

## Introduction

`logiBUS_AI_Raw_VT_AR_OPC` reads a local logiBUS analog input (`logiBUS_AI_IDA`), publishes the raw value via OPC-UA (`REAL`, 0..4095) and shows it in a VT number field (raw value, pure diagnostic display). In addition it is output as an `AR` plug, ready for further processing by a calibration block (`AR_CALIBRATE_2P_REF.X` or `AR_CALIBRATE_3P_REF.X`). The block was extracted from `logiBUS_AI_Calibrate_IDA_OPC` and `logiBUS_AI_Calibrate_3P_IDA_OPC`, where the combination `logiBUS_AI_IDA` + `AD_TO_AUDI` + `AUDI_SPLIT_2` + `Q_NumericValue_RAW` + `AUDI_TO_AR` occurred twice identically. Related: [`logiBUS_AI_Raw_Publish_OPC`](./logiBUS_AI_Raw_Publish_OPC.md).

![logiBUS_AI_Raw_VT_AR_OPC](logiBUS_AI_Raw_VT_AR_OPC.svg)

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

- **Input** (`logiBUS::io::AI::logiBUS_AI_S`): selection of the input `AnalogInput_I1`..`I8` (default `logiBUS_AI::Invalid`)
- **u16ObjId_RAWVAR** (`UINT`): object ID of the `CNumberVariable` (raw value 0..4095, uncalibrated, pure diagnostic display)
- **ID_RAW_WRITE** (`WSTRING`): raw value feedback address (`REAL` 0..4095, `ACTION=PUBLISH`)

### **Data Outputs**

None.

### **Adapters**

- **AR_OUT** (plug, `adapter::types::unidirectional::AR`): raw value as `AR`, ready for `CALIBRATE.X`

## Function Blocks (FBs) Used

- **`logiBUS_AI_IDA`** (`logiBUS::io::AI::logiBUS_AI_IDA`): parameters `QI = TRUE`, `AnalogInput_hysteresis = DWORD#50`, `TimeDelta = DWORD#250`, `TimeRateLimit = DWORD#100`
- **`AD_TO_AUDI`** (`adapter::conversion::unidirectional::AD_TO_AUDI`)
- **`AUDI_SPLIT_2`** (`adapter::events::unidirectional::AUDI_SPLIT_2`)
- **`Q_NumericValue_RAW`** (`isobus::UT::Q::Q_NumericValue_AUDI`): VT raw value display
- **`AUDI_TO_AR`** (`adapter::conversion::unidirectional::AUDI_TO_AR`)
- **`AR_SPLIT_2_Raw`** (`adapter::events::unidirectional::AR_SPLIT_2`)
- **`AR_PUBLISH_RAW`** (`adapter::net::AR_PUBLISH_1`, `QI = TRUE`): OPC-UA publish

## Functionality

- `Input` → `logiBUS_AI_IDA.Input`
- `logiBUS_AI_IDA.IN` → `AD_TO_AUDI.AD_IN` → `AUDI_SPLIT_2.IN`
- `AUDI_SPLIT_2.OUT1` → `Q_NumericValue_RAW.u32NewValue` (VT display); `u16ObjId_RAWVAR` → `Q_NumericValue_RAW.u16ObjId`
- `AUDI_SPLIT_2.OUT2` → `AUDI_TO_AR.AUDI_IN` → `AR_SPLIT_2_Raw.IN`
- `AR_SPLIT_2_Raw.OUT1` → `AR_OUT`
- `AR_SPLIT_2_Raw.OUT2` → `AR_PUBLISH_RAW.IN`; `ID_RAW_WRITE` → `AR_PUBLISH_RAW.ID`

## Technical Details

- The raw value is uncalibrated (0..4095); calibration takes place in the downstream block
- Two splitters (`AUDI_SPLIT_2`, `AR_SPLIT_2_Raw`), since an adapter must not branch to several targets
- Input parameters of `logiBUS_AI_IDA`: hysteresis 50, `TimeDelta` 250, `TimeRateLimit` 100

## Application Scenarios

- Showing the raw value of an analog input, publishing it via OPC-UA and feeding it to a calibration block

## Summary

`logiBUS_AI_Raw_VT_AR_OPC` delivers the analog raw value simultaneously to the VT, OPC-UA and an `AR` output.

---

### 🌐 Related topic pages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

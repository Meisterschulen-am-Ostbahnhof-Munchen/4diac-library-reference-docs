# logiBUS_PI_Raw_Publish_OPC

![logiBUS_PI_Raw_Publish_OPC_network](./logiBUS_PI_Raw_Publish_OPC_network.svg)

* * * * * * * * * *

## Introduction

`logiBUS_PI_Raw_Publish_OPC` reads a logiBUS pulse input (`logiBUS_PI_IDA`) purely as a **frequency (Hz, `REAL`)** and publishes it via OPC-UA - **without any VT interaction** and without channel enable/status. It is intended for speed sensors (pulse generators) whose physical pulse input sits on a station **without a VT connection** (e.g. pickup speed B03, hedgehog belt B10/B11 on APIXON STG2/STG3). The km/h conversion and VT display then run on the station with the VT connection, fed by a remote subscribe of this frequency (analogous to `logiBUS_AI_Raw_Publish_OPC`/`logiBUS_AI_Calibrate_Remote_IDA_OPC`).

![logiBUS_PI_Raw_Publish_OPC](logiBUS_PI_Raw_Publish_OPC.svg)

## Interface Structure

### **Event Inputs**

*No direct event inputs*

### **Event Outputs**

*No direct event outputs*

### **Data Inputs**

- **Input** (`logiBUS::io::PI::logiBUS_PI_S`, default `logiBUS_PI::Invalid`): Identifies the pulse input `PulseInput_I1..I9`
- **ID_HZ_WRITE** (`WSTRING`): Frequency publish address (`REAL` Hz, `ACTION=PUBLISH`), local on this station
- **stObjFilterTimeS** (`NumericObjectPool_S`): Object ID + scale/offset/decimals of the `CNumberVariable` for the PT1 filter time in seconds (VT input field)
- **ID_FILTER_TIME_READ** (`WSTRING`): Remote subscribe address (`REAL` seconds, `ACTION=SUBSCRIBE`) for the web override of the filter time
- **ID_FILTER_TIME_WRITE** (`WSTRING`): Local echo/re-publish address (`REAL` seconds, `ACTION=PUBLISH`) of the current filter time
- **sSection** (`STRING`): INI section in `settings.ini` for the persisted filter time (unique per sensor instance)
- **RASTER_HZ** (`REAL`, default `0.0001`): Rounding grid (Hz) before `AR_PUBLISH_HZ`. The default corresponds to practically no rounding (unchanged for existing callers); set via parameter to the desired resolution (e.g. `0.00208333` = 0.125 rpm at `SCALE=60` Hz → rpm downstream)

### **Data Outputs**

*No data outputs*

### **Adapters**

This function block has no adapters at its interface; `AR`/`AD` adapters are used internally.

## Function Blocks (FBs) Used

- **logiBUS_PI_IDA**: `logiBUS::io::PI::logiBUS_PI_IDA` (`QI=TRUE`, `ImpulseDelta=100`, `TimeDelta=250`, `TimeRateLimit=100`) - pulse input as an `AD` adapter
- **AD_TO_AR_NUM**: `adapter::conversion::unidirectional::AD_TO_AR_NUM` - converts the count value to `REAL`
- **FT_DERIV_10_AR**: `OSCAT_adapter::Control::FT_DERIV_10_AR` (`Faktor=1.0`, `run=TRUE`) - derivative (pulses per time = frequency)
- **LIT_DerivK**: `initval_AUI` (`INIT_VAL=9`) - constant `K` of the derivative
- **FT_PT1_AR**: `OSCAT_adapter::Control::FT_PT1_AR` (`K=1.0`) - PT1 filter
- **Round_Hz**: `OSCAT_adapter::Mathematical::ROUND_STEP_AR` - rounds to the grid `RASTER_HZ`
- **AR_PUBLISH_HZ**: [`adapter::net::AR_PUBLISH_1`](../../adapter/net/AR_PUBLISH_1.md) (`QI=TRUE`) - publishes the frequency
- **FilterTimeS**: SubApp `MyLib::sys::NumericValue_TO_AR2_OPC` - filter time from VT input field or OPC-UA
- **INI_FilterTimeS**: `eclipse4diac::storage::INI_AR2` (`QI=TRUE`, `SETM=TRUE`, `KEY='FilterTimeS'`, default `2.0`) - persists the filter time
- **ToATM**: SubApp `MyLib::sys::AR_S_TO_ATM` - converts seconds into an `ATM` time value

## Program Flow and Connections

1. `Input` → `logiBUS_PI_IDA.Input`; `logiBUS_PI_IDA.IN` → `AD_TO_AR_NUM.AD_IN`.
2. `AD_TO_AR_NUM.AR_OUT` → `FT_DERIV_10_AR.AR_IN`; `LIT_DerivK.OUT` → `FT_DERIV_10_AR.K`.
3. `FT_DERIV_10_AR.AR_OUT` → `FT_PT1_AR.AR_IN`.
4. Filter time: `FilterTimeS.VALUEO` → `INI_FilterTimeS.VAL` (persistence); `FilterTimeS.VALUE_OUT` → `ToATM.IN1`; `ToATM.OUT` → `FT_PT1_AR.TM`.
5. `FT_PT1_AR.AR_OUT` → `Round_Hz.AR_IN` (`RASTER_HZ` → `Round_Hz.RASTER`) → `AR_PUBLISH_HZ.IN`.
6. `ID_HZ_WRITE` → `AR_PUBLISH_HZ.ID`.

## Technical Details

- No VT interaction for the frequency itself, no channel enable and no status.
- The filter time is INI-persisted and can be changed via the VT input field or OPC-UA.
- `RASTER_HZ` can be used to limit the resolution in order to avoid unnecessary publish events.

## Application Scenarios

- Speed/rotation sensors on stations without a VT, whose frequency is transmitted via OPC-UA to the VT station (e.g. complemented by `AR_HZ_TO_Q_Publish_OPC` on the VT station)

## Summary

`logiBUS_PI_Raw_Publish_OPC` produces a filtered, rounded frequency in Hz from a pulse input and publishes it via OPC-UA.

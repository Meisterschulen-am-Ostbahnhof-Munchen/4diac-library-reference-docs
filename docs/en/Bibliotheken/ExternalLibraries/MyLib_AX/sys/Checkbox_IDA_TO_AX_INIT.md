# Checkbox_IDA_TO_AX_INIT

![Checkbox_IDA_TO_AX_INIT_network](./Checkbox_IDA_TO_AX_INIT_network.svg)

* * * * * * * * * *

## Introduction

`Checkbox_IDA_TO_AX_INIT` passes the state of a VT checkbox on live as an `AX` level (`isobus::UT::io::NumericValue::NumericValue_IDA` + `AD_TO_AX`). It is a pure on/off state without scaling or persistence, e.g. for `Automatik_Toggle.PERMIT`. The state is held via an edge detector and an SR flip-flop with a defined initial value (`_INIT`).

**Important:** `u16ObjId` is the ObjectID of the `CNumberVariable` that the checkbox references as its value store (e.g. `NumberVariable_Bunker_Fuellstand_Hoehe_Automatik`) - **not** the ObjectID of the checkbox (`CInputBoolean`) itself. The VT reports value changes under the NumberVariable ID (verified live).

![Checkbox_IDA_TO_AX_INIT](Checkbox_IDA_TO_AX_INIT.svg)

## Function Blocks (FBs) Used

### Sub-blocks: Checkbox_IDA_TO_AX_INIT

- **Type**: SubAppType
- **Internal FBs used**:
    - **NumericValue_IDA**: `isobus::UT::io::NumericValue::NumericValue_IDA` (`QI=TRUE`) - reads the raw value by ObjectID
    - **AD_TO_AX**: `adapter::conversion::unidirectional::AD_TO_AX` - converts the `DWORD` value into an `AX` signal (value `<> 0`)
    - **AX_RF_TRIG**: `adapter::events::unidirectional::AX_RF_TRIG` - detects rising (`ER`) and falling (`EF`) edge
    - **AX_SR_SYM_INIT**: `adapter::events::unidirectional::AX_SR_SYM_INIT` (`QI=TRUE`, `Q_INIT=TRUE`) - flip-flop with a defined initial value
- **Import**: `isobus::UT::Q::const::IDs::ID_NULL` (initial value of `u16ObjId`)

## Interface Structure

### **Event Inputs / Event Outputs**

None - events travel via the adapters.

### **Data Inputs**

- **u16ObjId** (`UINT`, initial value `ID_NULL`): Object ID of the VT checkbox, i.e. of the NumberVariable it references

### **Data Outputs**

None.

### **Adapters**

- **PERMIT_OUT** (plug): `adapter::types::unidirectional::AX` - raw checkbox state (0/1) as a level

## Program Flow and Connections

- `u16ObjId` → `NumericValue_IDA.u16ObjId`
- `NumericValue_IDA.IN` → `AD_TO_AX.AD_IN`
- `AD_TO_AX.AX_OUT` → `AX_RF_TRIG.QI`
- `AX_RF_TRIG.ER` → `AX_SR_SYM_INIT.S` (checkbox set)
- `AX_RF_TRIG.EF` → `AX_SR_SYM_INIT.R` (checkbox cleared)
- `AX_SR_SYM_INIT.Q` → `PERMIT_OUT`

## Technical Details

- Initial value: `AX_SR_SYM_INIT` is parameterised with `Q_INIT=TRUE`
- The checkbox is read raw by ObjectID; there is no scaling and no persistence
- Version 1.0 (2026-09-27): acts like an `AX_PERMIT` to gate the entire on/off of the filling automation

## Application Scenarios

- Enabling (`PERMIT`) an automation via a VT checkbox, e.g. `Automatik_Toggle.PERMIT`

## Summary

`Checkbox_IDA_TO_AX_INIT` reads a VT checkbox via its NumberVariable and provides the state as an `AX` level with a defined initial value.

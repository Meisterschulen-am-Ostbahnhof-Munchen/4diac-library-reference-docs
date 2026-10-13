# Checkbox_IDA_TO_AX

![Checkbox_IDA_TO_AX_network](./Checkbox_IDA_TO_AX_network.svg)

* * * * * * * * * *

## Introduction

`Checkbox_IDA_TO_AX` passes the state of a **VT checkbox on live as an AX level** (`isobus::UT::io::NumericValue::NumericValue_IDA` + `AD_TO_AX`). It is a pure on/off state without scaling and persistence, e.g. for `Automatik_Toggle.PERMIT`.

**Important:** `u16ObjId` is the object ID of the **`CNumberVariable`** that the checkbox references as its value store (e.g. `NumberVariable_Bunker_Fuellstand_Hoehe_Automatik`) - **not** the object ID of the checkbox (`CInputBoolean`) itself. The VT reports value changes under the NumberVariable ID (verified live).

![Checkbox_IDA_TO_AX](Checkbox_IDA_TO_AX.svg)

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

- **u16ObjId** (`UINT`): object ID of the number variable that the VT checkbox (InputBoolean) uses as its value store (default `ID_NULL`)

### **Data Outputs**

None.

### **Adapters**

- **PERMIT_OUT** (plug, `adapter::types::unidirectional::AX`): raw checkbox state (0/1) as a level

## Function Blocks (FBs) Used

- **`NumericValue_IDA`** (`isobus::UT::io::NumericValue::NumericValue_IDA`): parameter `QI = TRUE`, delivers the value of the number variable as an `AD` adapter
- **`AD_TO_AX`** (`adapter::conversion::unidirectional::AD_TO_AX`): converts the value to an `AX` level

## Functionality

- `u16ObjId` → `NumericValue_IDA.u16ObjId`
- `NumericValue_IDA.IN` → `AD_TO_AX.AD_IN`
- `AD_TO_AX.AX_OUT` → `PERMIT_OUT`

## Technical Details

- Level only; no scaling, no persistence
- Variant with initialisation: [Checkbox_IDA_TO_AX_INIT_Force1](./Checkbox_IDA_TO_AX_INIT_Force1.md)

## Application Scenarios

- Permit (`PERMIT`) of an automatic function via a checkbox in the VT, e.g. for `Automatik_Toggle`

## Summary

`Checkbox_IDA_TO_AX` turns a VT checkbox into an `AX` level for further logic.

---

### 🌐 Related topic pages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

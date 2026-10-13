# AUI_FIELDBUS_UINT_TO_AR

![AUI_FIELDBUS_UINT_TO_AR_network](./AUI_FIELDBUS_UINT_TO_AR_network.svg)

* * * * * * * * * *

## Introduction

`AUI_FIELDBUS_UINT_TO_AR` converts an `AUI` raw value (TECU, `UINT`) into an `AR` value and filters out the fieldbus special values `NOT_AVAILABLE` and `ERROR_INDI` on the way. The conversion is unscaled (`SCALE = 1.0`, `OFFSET = 0` fixed). The block was extracted because the same parameterisation was previously set individually in `TECU_RPTO`, `TECU_FPTO`, `TECU_COGSOG`, `TECU_GBSD`, `TECU_WBSD`, `TECU_VDS`, `TECU_FHS` and `TECU_RHS`. It lives in `MyLib_AX::sys` because it is generic and not TECU-specific.

![AUI_FIELDBUS_UINT_TO_AR](AUI_FIELDBUS_UINT_TO_AR.svg)

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

- **IN** (socket, `adapter::types::unidirectional::AUI`): TECU raw value
- **OUT** (plug, `adapter::types::unidirectional::AR`): `0.0` on `NOT_AVAILABLE`/`ERROR_INDI`, otherwise `IN` unscaled

## Function Blocks (FBs) Used

- **`SCALE_1`** (`logiBUS::signalprocessing::fieldbus::AUI_FIELDBUS_UINT_TO_SIGNAL_SCALED`): parameters `SCALE = 1.0`, `OFFSET = 0`

## Functionality

`IN` → `SCALE_1.IN` and `SCALE_1.OUT` → `OUT`. The filtering of the fieldbus special values (`VALID_SIGNAL_W`/`ERROR_INDI_W`/`NOT_AVAILABLE_W`, defined in `FIELDBUS_SIGNAL.gcf`) is done by the internal block; the SubApp only fixes scale and offset.

## Technical Details

- No event or data ports of its own, adapters only
- Fixed scale factor 1.0 and offset 0 (generic, unscaled variant)

## Application Scenarios

- Taking TECU raw values (e.g. speeds) from the fieldbus as an `AR` signal without scaling

## Summary

`AUI_FIELDBUS_UINT_TO_AR` wraps `AUI_FIELDBUS_UINT_TO_SIGNAL_SCALED` with `SCALE = 1.0`/`OFFSET = 0` as a reusable SubApp.

## 🛠️ Related Exercises

- [Uebung_078_AUI](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_078_AUI/)

---

### 🌐 Related topic pages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

# logiBUS_AI_Calibrate_3P_Subscribe_VT_OPC

* * * * * * * * * *

## Introduction

`logiBUS_AI_Calibrate_3P_Subscribe_VT_OPC` connects a subscibed raw-value data source to a full VT- and OPC-UA-backed 3-point calibration (`AR_CALIBRATE_3P_REF`) — used by the 3-point calibration training sample [InputOutputTesterButton_AI_Calibrate_3P_Subscribe_VT_OPC](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Meins/InputOutputTester/Button_AI_Calibrate_3P_Subscribe_VT_OPC/InputOutputTesterButton_AI_Calibrate_3P_Subscribe_VT_OPC/). The raw value is converted into a physically scaled value using piecewise linear interpolation across 3 reference points (`Min`, `Mid`, `Max`). Measured raw values (`MinRaw`, `MidRaw`, `MaxRaw`) are monitored live, exposed via VT and OPC-UA, and persisted in an INI file. In partial calibration (e.g., only 1 or 2 points calibrated), fallback logic immediately utilizes available reference points.

## Function blocks used

- **SUBSCRIBE_1** (`net::SUBSCRIBE_1`): receives the sensor raw value over the network.
- **AR_CALIBRATE_3P_REF** (adapter composite): 3-point calibration (`Min`/`Mid`/`Max`) with raw value diagnostics (`MinRaw`/`MidRaw`/`MaxRaw`), piecewise linear interpolation, partial calibration fallback logic, and configurable clipping (`xClipping`).
- **VT and OPC-UA bridges** (analogous to [`NumericValue_TO_AR2_OPC`](./NumericValue_TO_AR2_OPC.md)/[`OPC_TO_AR2`](./OPC_TO_AR2.md) and [`INI_IN_AND_STORE_AR2`](./INI_IN_AND_STORE_AR2.md)): expose `Min`/`Mid`/`Max` and `MinRaw`/`MidRaw`/`MaxRaw` both via VT input fields/displays and via OPC-UA, and persist calibration values in an INI file.

## Summary

Complete 3-point calibration block for subscribed raw values: raw-value reception over network, 3-point piecewise calibration with raw-value diagnostics, immediate partial calibration fallback, VT display/input, and OPC-UA connectivity in one composite.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

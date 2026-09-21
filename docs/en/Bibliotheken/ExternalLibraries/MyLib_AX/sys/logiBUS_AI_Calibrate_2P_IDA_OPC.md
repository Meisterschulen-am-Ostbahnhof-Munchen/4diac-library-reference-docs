# logiBUS_AI_Calibrate_2P_IDA_OPC

![logiBUS_AI_Calibrate_2P_IDA_OPC_network](./logiBUS_AI_Calibrate_2P_IDA_OPC_network.svg)

* * * * * * * * * *

## Introduction

`logiBUS_AI_Calibrate_2P_IDA_OPC` connects a physical analog input (`logiBUS_AI_IDA`) to a full VT- and OPC-UA-backed 2-point calibration (`AR_CALIBRATE_2P_REF`) — used by the [AI_Calibrate_2P training sample](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Meins/InputOutputTester/Button_AI_Calibrate_2P_OPC_UA/InputOutputTesterButton_AI_Calibrate_2P_OPC_UA/). The input's raw value is converted into a physically scaled value through the 2-point calibration chain; zero point (`Zero`) and span (`Span`) as well as their measured raw values (`ZeroRaw`/`SpanRaw`) are adjustable and readable via both VT and OPC-UA, and are persisted in an INI file. In partial calibration (only one point calibrated), a clean fallback logic immediately utilizes the available reference value.

## Function blocks used

- **logiBUS_AI_IDA** (`logiBUS::io::AI::logiBUS_AI_IDA`): physical analog input, provides the raw value as an adapter.
- **AR_CALIBRATE_2P_REF** (adapter composite): 2-point calibration (zero/span) with raw value diagnostics (`ZeroRaw`/`SpanRaw`), partial calibration fallback logic, and configurable clipping (`xClipping`).
- **VT and OPC-UA bridges** (analogous to [`NumericValue_TO_AR2_OPC`](./NumericValue_TO_AR2_OPC.md)/[`OPC_TO_AR2`](./OPC_TO_AR2.md) and [`INI_IN_AND_STORE_AR2`](./INI_IN_AND_STORE_AR2.md)): expose `Zero`/`Span` and `ZeroRaw`/`SpanRaw` both via VT input fields/displays and via OPC-UA, and persist calibration values in an INI file.

## Summary

Complete 2-point calibration block for a physical analog input: raw-value acquisition, 2-point calibration with raw-value diagnostics, immediate partial calibration fallback, VT display/input, and OPC-UA connectivity in one composite.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

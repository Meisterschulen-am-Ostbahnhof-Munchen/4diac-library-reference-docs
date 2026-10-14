# Softkey_IXA_TO_logiBUS_QXA

![Softkey_IXA_TO_logiBUS_QXA_network](./Softkey_IXA_TO_logiBUS_QXA_network.svg)

* * * * * * * * * *

## Introduction

`Softkey_IXA_TO_logiBUS_QXA` connects a VT softkey (`Softkey_IXA`) directly to a physical digital output (`logiBUS_QXA`) — functionally identical to [`Button_IXA_TO_logiBUS_QXA`](./Button_IXA_TO_logiBUS_QXA.md), but for softkeys instead of buttons.

![Softkey_IXA_TO_logiBUS_QXA](Softkey_IXA_TO_logiBUS_QXA.svg)

## Function Blocks (FBs) Used

### Sub-blocks: Softkey_IXA_TO_logiBUS_QXA

- **Type**: SubAppType
- **Internal FBs used**:
    - **Softkey_IXA**: `isobus::UT::io::Softkey::Softkey_IXA` — VT softkey adapter.
    - **logiBUS_QXA**: `logiBUS::io::DQ::logiBUS_QXA` — physical digital output.
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_2` — combines `logiBUS_QXA.QO` and `Softkey_IXA.QO` into the SubApp output `QO`.
- **Interface**: event input `INIT`, event output `INITO`, data input `QI` (default `TRUE`, passed on to both FBs), data output `QO`.
- **Functionality**: Direct adapter connection `Softkey_IXA.IN` → `logiBUS_QXA.OUT`, no intermediate logic (apart from the `AND_QO` combination of the quality signals).

## Program Flow and Connections

1. `u16ObjId` → `Softkey_IXA.u16ObjId`; `Output` → `logiBUS_QXA.Output`.
2. `Softkey_IXA.IN` → `logiBUS_QXA.OUT`.
3. Initialization: `INIT` → `logiBUS_QXA.INIT`; `logiBUS_QXA.INITO` → `Softkey_IXA.INIT`; `Softkey_IXA.INITO` → `INITO` and → `AND_QO.REQ`.
4. Quality: `QI` → `logiBUS_QXA.QI` and `Softkey_IXA.QI`; `logiBUS_QXA.QO` → `AND_QO.IN1`, `Softkey_IXA.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`.

## Application Scenarios

- Minimal softkey-to-output block with no status display or remote control.

## Summary

Softkey counterpart to `Button_IXA_TO_logiBUS_QXA` — identical pattern, different control type.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

# Button_IX_TO_logiBUS_QX

![Button_IX_TO_logiBUS_QX_network](./Button_IX_TO_logiBUS_QX_network.svg)

* * * * * * * * * *

## Introduction

`Button_IX_TO_logiBUS_QX` is the test_B counterpart to [`Button_IXA_TO_logiBUS_QXA`](../../MyLib_AX/sys/Button_IXA_TO_logiBUS_QXA.md): a VT button (`Button_IX`) directly switches a physical digital output (`logiBUS_QX`) — here without adapters, using classic event/data connections.

![Button_IX_TO_logiBUS_QX](Button_IX_TO_logiBUS_QX.svg)

## Function Blocks (FBs) Used

### Sub-blocks: Button_IX_TO_logiBUS_QX

- **Type**: SubAppType
- **Internal FBs used**:
    - **Button_IX**: `isobus::UT::io::Button::Button_IX` — VT button (non-adapter variant).
    - **logiBUS_QX**: `logiBUS::io::DQ::logiBUS_QX` — physical digital output (non-adapter variant).
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_2` — combines `logiBUS_QX.QO` and `Button_IX.QO` into the `QO` of the SubApp.
- **Functionality**: `Button_IX.IND` triggers `logiBUS_QX.REQ`; the data value `Button_IX.IN` is wired directly to `logiBUS_QX.OUT`.

## Program Flow and Connections

1. `u16ObjId` → `Button_IX.u16ObjId`; `Output` → `logiBUS_QX.Output`.
2. `Button_IX.IND` → `logiBUS_QX.REQ`; `Button_IX.IN` → `logiBUS_QX.OUT`.
3. **Initialization**: event input `INIT` (`EInit`) → `logiBUS_QX.INIT`; `logiBUS_QX.INITO` → `Button_IX.INIT`; `Button_IX.INITO` → event output `INITO` (`EInit`) and → `AND_QO.REQ`.
4. **Quality**: data input `QI` (`BOOL`, default `TRUE`) → `logiBUS_QX.QI` and `Button_IX.QI`; `logiBUS_QX.QO` → `AND_QO.IN1`; `Button_IX.QO` → `AND_QO.IN2`; `AND_QO.OUT` → data output `QO` (`BOOL`).

## Application Scenarios

- Minimal VT-button-to-output block for test_B, with no status display.

## Summary

test_B counterpart to `Button_IXA_TO_logiBUS_QXA`: same function, classic event/data connections instead of adapters.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

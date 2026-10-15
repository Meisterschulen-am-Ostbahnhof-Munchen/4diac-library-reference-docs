# Button_IXA_TO_logiBUS_QXA

![Button_IXA_TO_logiBUS_QXA_network](./Button_IXA_TO_logiBUS_QXA_network.svg)

* * * * * * * * * *

## Introduction

`Button_IXA_TO_logiBUS_QXA` connects a VT button (`Button_IXA`) directly to a physical digital output (`logiBUS_QXA`) — the simplest form of a VT-switchable output, with no status display and no OPC-UA. For the variant with a VT status color, see [`Button_IXA_TO_logiBUS_QXA_BG`](./Button_IXA_TO_logiBUS_QXA_BG.md).

![Button_IXA_TO_logiBUS_QXA](Button_IXA_TO_logiBUS_QXA.svg)

## Function Blocks (FBs) Used

### Sub-blocks: Button_IXA_TO_logiBUS_QXA

- **Type**: SubAppType
- **Internal FBs used**:
    - **Button_IXA**: `isobus::UT::io::Button::Button_IXA` — VT button adapter, `QI` comes from the input `QI`, `u16ObjId` identifies the VT button.
    - **logiBUS_QXA**: `logiBUS::io::DQ::logiBUS_QXA` — physical digital output, `QI` comes from the input `QI`.
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_2` — combines `logiBUS_QXA.QO` and `Button_IXA.QO` into the output `QO`.
- **Functionality**: The button's adapter output is wired directly to the physical output's adapter input — no intermediate logic. In addition, the SubApp has the interface `INIT`/`INITO` (events) and `QI`/`QO` (BOOL): `INIT` starts the init chain, `QI` (initial value `TRUE`) is passed on to both FBs, `QO` is the AND combination of their `QO` outputs.

## Program Flow and Connections

1. `u16ObjId` → `Button_IXA.u16ObjId`; `Output` → `logiBUS_QXA.Output`.
2. `Button_IXA.IN` (adapter) → `logiBUS_QXA.OUT` (adapter) — direct pass-through.
3. `INIT` → `logiBUS_QXA.INIT`; `logiBUS_QXA.INITO` → `Button_IXA.INIT`; `Button_IXA.INITO` → `INITO` and `AND_QO.REQ`.
4. `QI` → `logiBUS_QXA.QI` and `Button_IXA.QI`.
5. `logiBUS_QXA.QO` → `AND_QO.IN1`; `Button_IXA.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`.

## Application Scenarios

- Minimal VT-button-to-output block for exercises that don't yet need a status display or remote control.

## Summary

The simplest block variant in this family: a VT button wired directly to a physical output, with no additional function.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

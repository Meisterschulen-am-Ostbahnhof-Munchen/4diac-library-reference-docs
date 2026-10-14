# logiBUS_IXA_BG

![logiBUS_IXA_BG_network](./logiBUS_IXA_BG_network.svg)

* * * * * * * * * *

## Introduction

`logiBUS_IXA_BG` connects a physical digital input (`logiBUS_IXA`) to a VT status indication via the background-color family `GreenWhiteBackground1_AX` (see [Background Color Blocks (shared pattern)](./Background-Color-Blocks.md)) — the current state of the input is made visible on the VT as a green/white background, with no OPC-UA connection. For the variant with additional OPC-UA feedback, see [`logiBUS_IXA_BG_OPC`](./logiBUS_IXA_BG_OPC.md).

![logiBUS_IXA_BG](logiBUS_IXA_BG.svg)

## Function Blocks Used

### Sub-blocks: logiBUS_IXA_BG

- **Type**: SubAppType
- **Interface (additional)**: event input `INIT`, event output `INITO` (each `EInit`), data input `QI` (`BOOL`, default `TRUE`), data output `QO` (`BOOL`).
- **Internal FBs used**:
    - **logiBUS_IXA**: `logiBUS::io::DI::logiBUS_IXA` — physical digital input, adapter output `IN`; `QI` comes from the subapp input `QI`.
    - **GreenWhiteBackground1_AX** (SubApp): `MyLib::sys::GreenWhiteBackground1_AX` — single-object background-color block from the Background Color family, drives the VT object background color from `DI1`.
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_2` — combines the `QO` outputs of `logiBUS_IXA` and `GreenWhiteBackground1_AX` into `QO`.
- **Operation**: The physical input's adapter output is wired directly to the `DI1` adapter input of the background-color block, which handles the color switch of a VT object.

## Program Flow and Connections

1. `Input` → `logiBUS_IXA.Input`; `u16ObjId` → `GreenWhiteBackground1_AX.u16ObjId`.
2. `logiBUS_IXA.IN` (adapter) → `GreenWhiteBackground1_AX.DI1` (adapter).
3. Initialization: `INIT` → `logiBUS_IXA.INIT`; `logiBUS_IXA.INITO` → `GreenWhiteBackground1_AX.INIT`; `GreenWhiteBackground1_AX.INITO` → `INITO` and → `AND_QO.REQ`.
4. Quality: `QI` → `logiBUS_IXA.QI` and `GreenWhiteBackground1_AX.QI`; `logiBUS_IXA.QO` → `AND_QO.IN1`, `GreenWhiteBackground1_AX.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`.

## Application Scenarios

- Purely visual status indication of a physical digital input on the VT (e.g. limit switch state), where the value does not need to be reported externally via OPC-UA.

## Summary

Combines a physical digital input with the standard background-color family into a simple VT status indication.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

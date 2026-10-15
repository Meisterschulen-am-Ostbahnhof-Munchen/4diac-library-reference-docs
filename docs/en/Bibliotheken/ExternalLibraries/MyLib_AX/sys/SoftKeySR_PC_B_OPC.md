# SoftKeySR_PC_B_OPC

![SoftKeySR_PC_B_OPC_network](./SoftKeySR_PC_B_OPC_network.svg)

* * * * * * * * * *

## Introduction

`SoftKeySR_PC_B_OPC` is the device-B side (station 12, 192.168.1.12): receives set/reset each via its own `SERVER_0` method call (a pure RPC trigger, no value-change trick, no bridge needed), clocks the pure SR flip-flop logic (`AX_SR`, no toggle), drives `DigitalOutput_Q1`, and actively writes the new state back to device A via `AX_CLIENT_1_0`. Counterpart: [`SoftKeySR_PC_A_OPC`](./SoftKeySR_PC_A_OPC.md).

![SoftKeySR_PC_B_OPC](SoftKeySR_PC_B_OPC.svg)

## Function blocks used

- **TRIGGER_SET_SERVER / TRIGGER_RESET_SERVER** (`iec61499::net::SERVER_0`): each receives its own method call (`ID_SET_METHOD`/`ID_RESET_METHOD`).
- **AX_SR** (`adapter::events::unidirectional::AX_SR`): pure set/reset flip-flop logic, no toggle.
- **AX_SPLIT_2** (`adapter::events::unidirectional::AX_SPLIT_2`): splits the state to the physical output + the echo.
- **DigitalOutput_Q1** (`logiBUS::io::DQ::logiBUS_QXA`): physical output.
- **STATE_CLIENT** (`adapter::net::AX_CLIENT_1_0`): actively writes the state to device A under `ID_STATE_WRITE`.

## Initialization and quality

The SubApp has the event input `INIT`, the event output `INITO` (both `EInit`), the data input `QI` (`BOOL`, default `TRUE`) and the data output `QO` (`BOOL`). Initialization runs as a chain: `INIT` → `TRIGGER_SET_SERVER.INIT`, `TRIGGER_SET_SERVER.INITO` → `TRIGGER_RESET_SERVER.INIT`, `TRIGGER_RESET_SERVER.INITO` → `DigitalOutput_Q1.INIT`, `DigitalOutput_Q1.INITO` → `STATE_CLIENT.INIT`, `STATE_CLIENT.INITO` → `INITO` (and → `AND_QO.REQ`). `QI` is passed to `TRIGGER_SET_SERVER`, `TRIGGER_RESET_SERVER`, `DigitalOutput_Q1` and `STATE_CLIENT`. `QO` is the output of the instance `AND_QO` (`iec61131::booleanOperators::AND_BOOL_4`), which combines the `QO` of these four instances.

## Summary

Device-B side: receives set/reset via RPC, holds the pure SR flip-flop logic, and actively reports the state back.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

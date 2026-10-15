# AUI_GT_0

![AUI_GT_0_network](./AUI_GT_0_network.svg)

* * * * * * * * * *

## Introduction

`AUI_GT_0` checks whether a UINT adapter value is greater than 0 and outputs the result as a boolean AX adapter signal. A typical input is an object-ID or counter value (e.g. "how many channels are active"). The block is built identically for every numeric type (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![AUI_GT_0](AUI_GT_0.svg)

## Interface

- **Event input**: `INIT` (initialization)
- **Event output**: `INITO` (initialization finished, comes from the internal `initval_AUI`)
- **Socket**: `IN` (`AUI`) — the value compared against 0
- **Plug**: `AX_OUT` (`AX`) — TRUE when `IN > 0`

## Function Blocks (FBs) Used

### Sub-blocks: AUI_GT_0

- **Type**: SubAppType
- **Internal FBs used**:
    - **initval_AUI**: `adapter::types::unidirectional::AUI::initval::initval_AUI` — supplies the fixed comparison constant `UINT#0` as an adapter value (`INIT_VAL=UINT#0`). An adapter socket cannot take the constant as a parameter, so it is fed in through this block.
    - **AUI_GT**: `adapter::iec61131::comparison::AUI_GT` — adapter-native comparison `IN1 > IN2`.
- **Functionality**: `IN` → `AUI_GT.IN1`; constant 0 → `AUI_GT.IN2`; result → `AX_OUT`.

## Program Flow and Connections

1. `INIT` → `initval_AUI.INIT`; `initval_AUI.INITO` → `INITO`.
2. `IN` (adapter) → `AUI_GT.IN1`.
3. `initval_AUI.OUT` (constant `UINT#0`) → `AUI_GT.IN2`.
4. `AUI_GT.OUT` → `AX_OUT` (adapter).

## Application Scenarios

- Converting an object-ID or counter value (e.g. "how many channels are active") into a simple boolean adapter signal for VT status displays or enable logic.

## Summary

`AUI_GT_0` is a compact wrapper around the adapter-native comparison `AUI_GT` — converts "UINT > 0" directly into an AX adapter signal, so the caller does not have to create and wire the zero constant itself.

## 🛠️ Related Exercises

- [Uebung_071_AUI](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_071_AUI/)
- [Uebung_071a_AUI](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_071a_AUI/)

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

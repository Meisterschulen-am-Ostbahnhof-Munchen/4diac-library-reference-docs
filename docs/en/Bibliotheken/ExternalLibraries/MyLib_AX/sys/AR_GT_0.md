# AR_GT_0

![AR_GT_0_network](./AR_GT_0_network.svg)

* * * * * * * * * *

## Introduction

`AR_GT_0` checks whether a REAL adapter value is greater than 0 and outputs the result as a boolean AX adapter signal. A typical input is a floating-point value (REAL), e.g. a speed. The block is built identically for every numeric type (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![AR_GT_0](AR_GT_0.svg)

## Interface

- **Event input**: `INIT` (initialization)
- **Event output**: `INITO` (initialization finished, comes from the internal `LIT_0`)
- **Socket**: `IN` (`AR`) — the value compared against 0
- **Plug**: `AX_OUT` (`AX`) — TRUE when `IN > 0`

## Function Blocks (FBs) Used

### Sub-blocks: AR_GT_0

- **Type**: SubAppType
- **Internal FBs used**:
    - **LIT_0**: `adapter::types::unidirectional::AR::initval::initval_AR` — supplies the fixed comparison constant `0.0` as an adapter value (`INIT_VAL=0.0`). An adapter socket cannot take the constant as a parameter, so it is fed in through this block.
    - **AR_GT**: `adapter::iec61131::comparison::AR_GT` — adapter-native comparison `IN1 > IN2`.
- **Functionality**: `IN` → `AR_GT.IN1`; constant 0 → `AR_GT.IN2`; result → `AX_OUT`.

## Program Flow and Connections

1. `INIT` → `LIT_0.INIT`; `LIT_0.INITO` → `INITO`.
2. `IN` (adapter) → `AR_GT.IN1`.
3. `LIT_0.OUT` (constant `0.0`) → `AR_GT.IN2`.
4. `AR_GT.OUT` → `AX_OUT` (adapter).

## Application Scenarios

- Converting a floating-point value (REAL), e.g. a speed into a simple boolean adapter signal for VT status displays or enable logic.

## Summary

`AR_GT_0` is a compact wrapper around the adapter-native comparison `AR_GT` — converts "REAL > 0" directly into an AX adapter signal, so the caller does not have to create and wire the zero constant itself.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

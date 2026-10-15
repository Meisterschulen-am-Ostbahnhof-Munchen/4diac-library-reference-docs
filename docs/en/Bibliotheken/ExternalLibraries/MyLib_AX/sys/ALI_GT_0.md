# ALI_GT_0

![ALI_GT_0_network](./ALI_GT_0_network.svg)

* * * * * * * * * *

## Introduction

`ALI_GT_0` checks whether a LINT adapter value is greater than 0 and outputs the result as a boolean AX adapter signal. A typical input is a signed 64-bit value. The block is built identically for every numeric type (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![ALI_GT_0](ALI_GT_0.svg)

## Interface

- **Event input**: `INIT` (initialization)
- **Event output**: `INITO` (initialization finished, comes from the internal `initval_ALI`)
- **Socket**: `IN` (`ALI`) — the value compared against 0
- **Plug**: `AX_OUT` (`AX`) — TRUE when `IN > 0`

## Function Blocks (FBs) Used

### Sub-blocks: ALI_GT_0

- **Type**: SubAppType
- **Internal FBs used**:
    - **initval_ALI**: `adapter::types::unidirectional::ALI::initval::initval_ALI` — supplies the fixed comparison constant `LINT#0` as an adapter value (`INIT_VAL=LINT#0`). An adapter socket cannot take the constant as a parameter, so it is fed in through this block.
    - **ALI_GT**: `adapter::iec61131::comparison::ALI_GT` — adapter-native comparison `IN1 > IN2`.
- **Functionality**: `IN` → `ALI_GT.IN1`; constant 0 → `ALI_GT.IN2`; result → `AX_OUT`.

## Program Flow and Connections

1. `INIT` → `initval_ALI.INIT`; `initval_ALI.INITO` → `INITO`.
2. `IN` (adapter) → `ALI_GT.IN1`.
3. `initval_ALI.OUT` (constant `LINT#0`) → `ALI_GT.IN2`.
4. `ALI_GT.OUT` → `AX_OUT` (adapter).

## Application Scenarios

- Converting a signed 64-bit value into a simple boolean adapter signal for VT status displays or enable logic.

## Summary

`ALI_GT_0` is a compact wrapper around the adapter-native comparison `ALI_GT` — converts "LINT > 0" directly into an AX adapter signal, so the caller does not have to create and wire the zero constant itself.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

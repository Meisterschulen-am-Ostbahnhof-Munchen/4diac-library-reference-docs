# ADI_GT_0

![ADI_GT_0_network](./ADI_GT_0_network.svg)

* * * * * * * * * *

## Introduction

`ADI_GT_0` checks whether a DINT adapter value is greater than 0 and outputs the result as a boolean AX adapter signal. A typical input is a signed 32-bit value. The block is built identically for every numeric type (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![ADI_GT_0](ADI_GT_0.svg)

## Interface

- **Event input**: `INIT` (initialization)
- **Event output**: `INITO` (initialization finished, comes from the internal `initval_ADI`)
- **Socket**: `IN` (`ADI`) — the value compared against 0
- **Plug**: `AX_OUT` (`AX`) — TRUE when `IN > 0`

## Function Blocks (FBs) Used

### Sub-blocks: ADI_GT_0

- **Type**: SubAppType
- **Internal FBs used**:
    - **initval_ADI**: `adapter::types::unidirectional::ADI::initval::initval_ADI` — supplies the fixed comparison constant `DINT#0` as an adapter value (`INIT_VAL=DINT#0`). An adapter socket cannot take the constant as a parameter, so it is fed in through this block.
    - **ADI_GT**: `adapter::iec61131::comparison::ADI_GT` — adapter-native comparison `IN1 > IN2`.
- **Functionality**: `IN` → `ADI_GT.IN1`; constant 0 → `ADI_GT.IN2`; result → `AX_OUT`.

## Program Flow and Connections

1. `INIT` → `initval_ADI.INIT`; `initval_ADI.INITO` → `INITO`.
2. `IN` (adapter) → `ADI_GT.IN1`.
3. `initval_ADI.OUT` (constant `DINT#0`) → `ADI_GT.IN2`.
4. `ADI_GT.OUT` → `AX_OUT` (adapter).

## Application Scenarios

- Converting a signed 32-bit value into a simple boolean adapter signal for VT status displays or enable logic.

## Summary

`ADI_GT_0` is a compact wrapper around the adapter-native comparison `ADI_GT` — converts "DINT > 0" directly into an AX adapter signal, so the caller does not have to create and wire the zero constant itself.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

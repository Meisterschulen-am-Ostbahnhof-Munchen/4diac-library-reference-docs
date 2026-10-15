# AS_GT_0

![AS_GT_0_network](./AS_GT_0_network.svg)

* * * * * * * * * *

## Introduction

`AS_GT_0` checks whether a SINT adapter value is greater than 0 and outputs the result as a boolean AX adapter signal. A typical input is a signed 8-bit value. The block is built identically for every numeric type (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![AS_GT_0](AS_GT_0.svg)

## Interface

- **Event input**: `INIT` (initialization)
- **Event output**: `INITO` (initialization finished, comes from the internal `initval_AS`)
- **Socket**: `IN` (`AS`) — the value compared against 0
- **Plug**: `AX_OUT` (`AX`) — TRUE when `IN > 0`

## Function Blocks (FBs) Used

### Sub-blocks: AS_GT_0

- **Type**: SubAppType
- **Internal FBs used**:
    - **initval_AS**: `adapter::types::unidirectional::AS::initval::initval_AS` — supplies the fixed comparison constant `SINT#0` as an adapter value (`INIT_VAL=SINT#0`). An adapter socket cannot take the constant as a parameter, so it is fed in through this block.
    - **AS_GT**: `adapter::iec61131::comparison::AS_GT` — adapter-native comparison `IN1 > IN2`.
- **Functionality**: `IN` → `AS_GT.IN1`; constant 0 → `AS_GT.IN2`; result → `AX_OUT`.

## Program Flow and Connections

1. `INIT` → `initval_AS.INIT`; `initval_AS.INITO` → `INITO`.
2. `IN` (adapter) → `AS_GT.IN1`.
3. `initval_AS.OUT` (constant `SINT#0`) → `AS_GT.IN2`.
4. `AS_GT.OUT` → `AX_OUT` (adapter).

## Application Scenarios

- Converting a signed 8-bit value into a simple boolean adapter signal for VT status displays or enable logic.

## Summary

`AS_GT_0` is a compact wrapper around the adapter-native comparison `AS_GT` — converts "SINT > 0" directly into an AX adapter signal, so the caller does not have to create and wire the zero constant itself.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

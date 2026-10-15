# AUDI_GT_0

![AUDI_GT_0_network](./AUDI_GT_0_network.svg)

* * * * * * * * * *

## Introduction

`AUDI_GT_0` checks whether a UDINT adapter value is greater than 0 and outputs the result as a boolean AX adapter signal. A typical input is an unsigned 32-bit counter value. The block is built identically for every numeric type (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![AUDI_GT_0](AUDI_GT_0.svg)

## Interface

- **Event input**: `INIT` (initialization)
- **Event output**: `INITO` (initialization finished, comes from the internal `initval_AUDI`)
- **Socket**: `IN` (`AUDI`) — the value compared against 0
- **Plug**: `AX_OUT` (`AX`) — TRUE when `IN > 0`

## Function Blocks (FBs) Used

### Sub-blocks: AUDI_GT_0

- **Type**: SubAppType
- **Internal FBs used**:
    - **initval_AUDI**: `adapter::types::unidirectional::AUDI::initval::initval_AUDI` — supplies the fixed comparison constant `UDINT#0` as an adapter value (`INIT_VAL=UDINT#0`). An adapter socket cannot take the constant as a parameter, so it is fed in through this block.
    - **AUDI_GT**: `adapter::iec61131::comparison::AUDI_GT` — adapter-native comparison `IN1 > IN2`.
- **Functionality**: `IN` → `AUDI_GT.IN1`; constant 0 → `AUDI_GT.IN2`; result → `AX_OUT`.

## Program Flow and Connections

1. `INIT` → `initval_AUDI.INIT`; `initval_AUDI.INITO` → `INITO`.
2. `IN` (adapter) → `AUDI_GT.IN1`.
3. `initval_AUDI.OUT` (constant `UDINT#0`) → `AUDI_GT.IN2`.
4. `AUDI_GT.OUT` → `AX_OUT` (adapter).

## Application Scenarios

- Converting an unsigned 32-bit counter value into a simple boolean adapter signal for VT status displays or enable logic.

## Summary

`AUDI_GT_0` is a compact wrapper around the adapter-native comparison `AUDI_GT` — converts "UDINT > 0" directly into an AX adapter signal, so the caller does not have to create and wire the zero constant itself.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

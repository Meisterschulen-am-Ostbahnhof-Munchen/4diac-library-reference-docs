# AI_GT_0

![AI_GT_0_network](./AI_GT_0_network.svg)

* * * * * * * * * *

## Introduction

`AI_GT_0` checks whether a INT adapter value is greater than 0 and outputs the result as a boolean AX adapter signal. A typical input is a signed 16-bit value. The block is built identically for every numeric type (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![AI_GT_0](AI_GT_0.svg)

## Interface

- **Event input**: `INIT` (initialization)
- **Event output**: `INITO` (initialization finished, comes from the internal `initval_AI`)
- **Socket**: `IN` (`AI`) — the value compared against 0
- **Plug**: `AX_OUT` (`AX`) — TRUE when `IN > 0`

## Function Blocks (FBs) Used

### Sub-blocks: AI_GT_0

- **Type**: SubAppType
- **Internal FBs used**:
    - **initval_AI**: `adapter::types::unidirectional::AI::initval::initval_AI` — supplies the fixed comparison constant `INT#0` as an adapter value (`INIT_VAL=INT#0`). An adapter socket cannot take the constant as a parameter, so it is fed in through this block.
    - **AI_GT**: `adapter::iec61131::comparison::AI_GT` — adapter-native comparison `IN1 > IN2`.
- **Functionality**: `IN` → `AI_GT.IN1`; constant 0 → `AI_GT.IN2`; result → `AX_OUT`.

## Program Flow and Connections

1. `INIT` → `initval_AI.INIT`; `initval_AI.INITO` → `INITO`.
2. `IN` (adapter) → `AI_GT.IN1`.
3. `initval_AI.OUT` (constant `INT#0`) → `AI_GT.IN2`.
4. `AI_GT.OUT` → `AX_OUT` (adapter).

## Application Scenarios

- Converting a signed 16-bit value into a simple boolean adapter signal for VT status displays or enable logic.

## Summary

`AI_GT_0` is a compact wrapper around the adapter-native comparison `AI_GT` — converts "INT > 0" directly into an AX adapter signal, so the caller does not have to create and wire the zero constant itself.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

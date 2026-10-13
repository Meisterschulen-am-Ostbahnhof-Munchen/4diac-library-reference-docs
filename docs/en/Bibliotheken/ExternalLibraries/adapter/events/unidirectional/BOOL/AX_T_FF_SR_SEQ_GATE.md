# AX_T_FF_SR_SEQ_GATE

![AX_T_FF_SR_SEQ_GATE_ecc](./AX_T_FF_SR_SEQ_GATE_ecc.svg)

* * * * * * * * * *

## Introduction

The `AX_T_FF_SR_SEQ_GATE` is a toggle/set/reset flip-flop with bidirectional chain forwarding, like `AX_T_FF_SR_SEQ`, and additionally has an enable socket `GATE` (type `AX`) and a dedicated state `GATE_CLOSED`. While `GATE` is TRUE the block works normally. If `GATE` falls to FALSE, a RESET is forced immediately and the block is locked; the only way out of `GATE_CLOSED` is `GATE.E1[GATE.D1]`, i.e. reopening the gate.

![AX_T_FF_SR_SEQ_GATE](AX_T_FF_SR_SEQ_GATE.svg)

## Interface Structure

### **Event Inputs**

- **S**: sets the output `Q` (local, never acts on `CHAIN_IN`/`CHAIN_OUT`); has no effect during `GATE_CLOSED`
- **R**: resets the output `Q` (local); has no effect during `GATE_CLOSED`
- **CLK**: clock for toggling the output (local); has no effect during `GATE_CLOSED`

### **Event Outputs**

The function block has no direct event outputs.

### **Data Inputs**

The function block has no direct data inputs.

### **Data Outputs**

The function block has no direct data outputs.

### **Adapters**

- **Q** (plug, `adapter::types::unidirectional::AX`): value of the flip-flop
- **CHAIN_OUT** (plug, `adapter::types::bidirectional::ASR2`): forward Set/Reset to the next chain element (`EO_SET`/`EO_RESET`), backward Set/Reset received from the next chain element (`EI_SET`/`EI_RESET`); not during `GATE_CLOSED`
- **CHAIN_IN** (socket, `adapter::types::bidirectional::ASR2`): forward Set/Reset received from the previous chain element (`EO_SET`/`EO_RESET`), backward Set/Reset passed on to the previous chain element (`EI_SET`/`EI_RESET`); not during `GATE_CLOSED`
- **GATE** (socket, `adapter::types::unidirectional::AX`): enable, must be TRUE. If `GATE` falls to FALSE, `GATE_CLOSED` follows immediately (forced RESET and lock); the state is left only via `GATE.E1[GATE.D1]`

## Functionality

The ECC has the states `START`, `SET`, `RESET`, `SET_CHAIN`, `RESET_CHAIN`, `SET_CHAIN_BWD`, `RESET_CHAIN_BWD` and `GATE_CLOSED`. There are four strictly separate chain paths (forward set, forward reset, backward set, backward reset) without event duplication:

- **Local**: `S[GATE.D1]` or `CLK[GATE.D1]` leads to `SET` (algorithm `SET`: `Q.D1 := TRUE`, output `Q.E1`); `R` or `CLK` from a set state leads to `RESET` (`Q.D1 := FALSE`). These local events never act on the chain.
- **Forward**: `CHAIN_IN.EO_SET[GATE.D1]` leads to `SET_CHAIN` (sets `Q` and outputs `CHAIN_OUT.EO_SET`), `CHAIN_IN.EO_RESET` leads to `RESET_CHAIN` (resets `Q` and outputs `CHAIN_OUT.EO_RESET`).
- **Backward**: `CHAIN_OUT.EI_SET[GATE.D1]` leads to `SET_CHAIN_BWD` (sets `Q` and outputs `CHAIN_IN.EI_SET`), `CHAIN_OUT.EI_RESET` leads to `RESET_CHAIN_BWD` (resets `Q` and outputs `CHAIN_IN.EI_RESET`).
- **Gate**: every state except `GATE_CLOSED` has `[NOT GATE.D1]` to `GATE_CLOSED` as its first transition. There the algorithm `RESET` is executed with output `Q.E1`; `S`, `R`, `CLK`, `CHAIN_IN` and `CHAIN_OUT` have no effect because no transition is defined. The only way back to `RESET` is `GATE.E1[GATE.D1]`.

## Technical Details

- The gate check `[NOT GATE.D1]` is eventless and comes first in the transition order of every state so that it is evaluated before the unguarded event transitions (e.g. `R`, `CLK`, `CHAIN_IN.EO_RESET`)
- The lock is level-based rather than a mere edge pulse on `R`: while the gate is closed, the block cannot be switched on again
- Set requests (`S`, `CLK` for setting, `CHAIN_IN.EO_SET`, `CHAIN_OUT.EI_SET`) are accepted only when `GATE.D1 = TRUE`; reset paths may always pass
- Nothing is forwarded to `CHAIN_IN`/`CHAIN_OUT` during `GATE_CLOSED`

## State Overview

| State | Meaning |
| ----- | ------- |
| START | initial state; with `GATE.D1 = FALSE` it goes to `GATE_CLOSED` immediately |
| SET | `Q` = TRUE through local `S`/`CLK` |
| RESET | `Q` = FALSE through local `R`/`CLK` |
| SET_CHAIN | `Q` = TRUE, set came forward and is passed on to `CHAIN_OUT.EO_SET` |
| RESET_CHAIN | `Q` = FALSE, reset came forward and is passed on to `CHAIN_OUT.EO_RESET` |
| SET_CHAIN_BWD | `Q` = TRUE, set came backward and is passed on to `CHAIN_IN.EI_SET` |
| RESET_CHAIN_BWD | `Q` = FALSE, reset came backward and is passed on to `CHAIN_IN.EI_RESET` |
| GATE_CLOSED | locked, `Q` = FALSE; exit only via `GATE.E1[GATE.D1]` to `RESET` |

## Application Scenarios

- Switchable outputs with a higher-level, level-based safety shutdown (e.g. PTO monitoring) that may only be switched on again after the enable has been granted again
- Sequenced chains of several flip-flops in which one element is to be locked depending on an enable

## ⚖️ Comparison with Similar Blocks

Compared with `AX_T_FF_SR_SEQ` (same four chain paths without an enable), `AX_T_FF_SR_SEQ_GATE` additionally offers the socket `GATE` and the state `GATE_CLOSED`. The lock is truly level-based here and thus solves a problem that a pure edge pulse on `R` does not: immediate re-enabling despite a shutdown that is still present.

## Conclusion

The `AX_T_FF_SR_SEQ_GATE` extends the sequenceable AX flip-flop with a level-based enable: if `GATE` falls, the block is reset and locked immediately, and only a renewed `GATE.E1[GATE.D1]` releases it again.

# AX_T_FF_SR_SEQ

![AX_T_FF_SR_SEQ_ecc](./AX_T_FF_SR_SEQ_ecc.svg)

* * * * * * * * * *

## Introduction

`AX_T_FF_SR_SEQ` is an event-driven bistable flip-flop with toggle function (a copy of `AX_T_FF_SR`) that is additionally **truly bidirectionally chainable** via the adapter `ASR2` (`CHAIN_IN`/`CHAIN_OUT`). This allows several belts to be switched on and off in a staggered manner (e.g. together with [`ASR2_DELAY`](../../bidirectional/ASR2_DELAY.md)). The local events `S`/`R`/`CLK` behave exactly like in `AX_T_FF_SR` and **never** trigger `CHAIN_IN`/`CHAIN_OUT`.

![AX_T_FF_SR_SEQ](AX_T_FF_SR_SEQ.svg)

## Interface Structure

### **Event Inputs**

- **S**: Sets output `Q` (local, never affects `CHAIN_IN`/`CHAIN_OUT`)
- **R**: Resets output `Q` (local, never affects `CHAIN_IN`/`CHAIN_OUT`)
- **CLK**: Clock for toggling the output (local, never affects `CHAIN_IN`/`CHAIN_OUT`)

### **Event Outputs**

*No direct event outputs - events run via the adapter plugs*

### **Data Inputs**

*No data inputs*

### **Data Outputs**

*No data outputs - the state is output via the adapter `Q`*

### **Adapters**

- **Q** (plug): Unidirectional adapter of type `adapter::types::unidirectional::AX` - value of the flip-flop
- **CHAIN_IN** (socket): Bidirectional adapter of type `adapter::types::bidirectional::ASR2` - forward: receive set/reset from the previous chain element (`EO_SET`/`EO_RESET`); backward: pass set/reset on to the previous chain element (`EI_SET`/`EI_RESET`)
- **CHAIN_OUT** (plug): Bidirectional adapter of type `adapter::types::bidirectional::ASR2` - forward: pass set/reset on to the next chain element (`EO_SET`/`EO_RESET`); backward: receive set/reset from the next chain element (`EI_SET`/`EI_RESET`)

## How it works

The block has seven states (`START`, `SET`, `RESET`, `SET_CHAIN`, `RESET_CHAIN`, `SET_CHAIN_BWD`, `RESET_CHAIN_BWD`). On entering a set state the output `Q` is set to `TRUE` by the algorithm `SET`, in a reset state to `FALSE` by `RESET`, and output via `Q.E1`.

There are **four strictly separate, non-branching chain paths**:

1. `CHAIN_IN.EO_SET` → local `SET` + `CHAIN_OUT.EO_SET` (forward set)
2. `CHAIN_IN.EO_RESET` → local `RESET` + `CHAIN_OUT.EO_RESET` (forward reset)
3. `CHAIN_OUT.EI_SET` → local `SET` + `CHAIN_IN.EI_SET` (backward set)
4. `CHAIN_OUT.EI_RESET` → local `RESET` + `CHAIN_IN.EI_RESET` (backward reset)

A set arriving via path 1 never additionally triggers path 3 or 4 and vice versa - each state fires exactly one chain action. Locally:

- `S` or `CLK` in `START` or `RESET`/`RESET_CHAIN`/`RESET_CHAIN_BWD` → `SET`
- `R` or `CLK` in `SET`/`SET_CHAIN`/`SET_CHAIN_BWD` → `RESET`
- The four chain events are possible from every state and lead to the respective chain state.

## Technical Details

- Local `S`/`R`/`CLK` only changes `Q`; the chain is not triggered.
- With the backward paths (version 1.1) the same physical chain elements can be switched in a staggered manner both forward (e.g. set, downstream first) and backward (e.g. reset, upstream first), without a forward event also propagating backward or vice versa.
- An `R` in state `START` has no effect, since no transition with `R` exists from there.

## Application Scenarios

- Staggered switching on/off of several conveyor belts (e.g. hedgehog belt, elevator, cross belt) with delay via [`ASR2_DELAY`](../../bidirectional/ASR2_DELAY.md)

## Summary

`AX_T_FF_SR_SEQ` extends the set/reset/toggle flip-flop by four separate chain paths (forward/backward, set/reset) for cascaded, staggered switching operations.

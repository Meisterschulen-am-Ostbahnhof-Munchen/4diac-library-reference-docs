# ASRT_SRT_TO_SR_AE

![ASRT_SRT_TO_SR_AE](ASRT_SRT_TO_SR_AE.svg)

* * * * * * * * * *

## Introduction

The function block **ASRT_SRT_TO_SR_AE** is a composite FB that splits a unidirectional ASRT adapter (set/reset/toggle) into an ASR adapter (set/reset) and a separate AE adapter (toggle, pure event). It is the reverse of [ASRT_SR_AE_TO_SRT](ASRT_SR_AE_TO_SRT.md) and a mixed variant of [ASRT_SRT_TO_3AE](ASRT_SRT_TO_3AE.md).

## Interface Structure

### **Event Inputs**

None. Events are received exclusively via the adapter socket.

### **Event Outputs**

None. Events are forwarded exclusively via the adapter plugs.

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

| Role | Name | Type | Description |
| ------- | ------ | ----- | -------------- |
| Socket | `ASRT_IN` | `adapter::types::unidirectional::ASRT` | Incoming set/reset/toggle signal. |
| Plug | `SR_OUT` | `adapter::types::unidirectional::ASR` | Set/reset output. |
| Plug | `TOGGLE_OUT` | `adapter::types::unidirectional::AE` | Toggle / invert output. |

## Functionality

The block is a pure FBNetwork of three event connections with no algorithms of its own: `ASRT_IN.SET` is wired directly to `SR_OUT.SET`, `ASRT_IN.RESET` to `SR_OUT.RESET`, and `ASRT_IN.TOGGLE` to `TOGGLE_OUT.E1`. A SET, RESET, or TOGGLE event at the ASRT socket immediately triggers the matching event at `SR_OUT` or `TOGGLE_OUT`.

## Technical Features

- **Pure wiring**: A composite FB with no ECC or algorithms, consisting solely of three direct event connections.
- **Mixed adapter outputs**: Provides SET/RESET already as a ready-made ASR adapter instead of two individual AE plugs — useful when downstream logic already expects an ASR input (e.g. `ASR_AX_SR`) and only TOGGLE needs to be handled separately.

## State Overview

The function block has **no state machine**. Every `SET`, `RESET`, or `TOGGLE` event at `ASRT_IN` is forwarded immediately to `SR_OUT` or `TOGGLE_OUT`.

## Application Scenarios

- **Connecting to existing ASR interfaces**: An ASRT signal needs to be forwarded to a block that only expects SET/RESET as an ASR adapter, while TOGGLE is tapped off separately (e.g. for diagnostics).
- **Simplifying downstream logic** that already processes set/reset bundled as an ASR adapter.

## Comparison with Similar Components

- **[ASRT_SR_AE_TO_SRT](ASRT_SR_AE_TO_SRT.md)**: the reverse direction – merges ASR (SET/RESET) and AE (TOGGLE) into one ASRT signal.
- **[ASRT_SRT_TO_3AE](ASRT_SRT_TO_3AE.md)**: the same split, but with three individual AE plugs instead of a ready-made ASR adapter for SET/RESET.
- **[ASR_SR_TO_2AE](ASR_SR_TO_2AE.md)**: the `TOGGLE`-reduced variant for ASR.

## Conclusion

`ASRT_SRT_TO_SR_AE` is a simple, pure-wiring solution that splits an ASRT signal into an ASR adapter (SET/RESET) and a separate toggle event.

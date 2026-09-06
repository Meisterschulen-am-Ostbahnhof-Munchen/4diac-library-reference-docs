# ASRT_SR_AE_TO_SRT

![ASRT_SR_AE_TO_SRT](ASRT_SR_AE_TO_SRT.svg)

* * * * * * * * * *

## Introduction

The function block **ASRT_SR_AE_TO_SRT** is a composite FB that merges a unidirectional ASR adapter (set/reset) and a separate AE adapter (toggle, pure event) into one common ASRT adapter (set/reset/toggle). It is a mixed variant of [ASRT_3AE_TO_SRT](ASRT_3AE_TO_SRT.md): instead of three individual AE sockets for SET, RESET, and TOGGLE, it accepts SET/RESET already as a ready-made ASR adapter and only adds TOGGLE separately.

## Interface Structure

### **Event Inputs**

None. Events are received exclusively via the adapter sockets.

### **Event Outputs**

None. Events are forwarded exclusively via the adapter plug.

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

| Role | Name | Type | Description |
| ------- | ------ | ----- | -------------- |
| Socket | `SR_IN` | `adapter::types::unidirectional::ASR` | Incoming set/reset signal. |
| Socket | `TOGGLE_IN` | `adapter::types::unidirectional::AE` | Toggle / invert output. |
| Plug | `ASRT_OUT` | `adapter::types::unidirectional::ASRT` | Merged set/reset/toggle signal. |

## Functionality

The block is a pure FBNetwork of three event connections with no algorithms of its own: `SR_IN.SET` is wired directly to `ASRT_OUT.SET`, `SR_IN.RESET` to `ASRT_OUT.RESET`, and `TOGGLE_IN.E1` to `ASRT_OUT.TOGGLE`. A SET or RESET event at the ASR socket, or an event at the AE socket, immediately triggers the matching event at the ASRT plug.

## Technical Features

- **Pure wiring**: A composite FB with no ECC or algorithms, consisting solely of three direct event connections.
- **Mixed adapter inputs**: Combines an already-complete ASR adapter (SET/RESET) with a separate AE adapter (TOGGLE) — useful when SET/RESET already exist as an ASR signal (e.g. from [ASR_2AE_TO_SR](ASR_2AE_TO_SR.md) or an existing ASR network branch) and only TOGGLE needs to be added.

## State Overview

The function block has **no state machine**. Every event at `SR_IN` (SET/RESET) or `TOGGLE_IN` is forwarded immediately as the matching event type to `ASRT_OUT`.

## Application Scenarios

- **Retrofitting TOGGLE**: An existing ASR network should be extended with a toggle function without rewiring SET/RESET.
- **Reusing existing ASR signal paths** in combination with a separate toggle source, e.g. a button via [ASR_MERGE_2](../../../events/unidirectional/EVENT/ASR_MERGE_2.md).

## Comparison with Similar Components

- **[ASRT_SRT_TO_SR_AE](ASRT_SRT_TO_SR_AE.md)**: the reverse direction – splits an ASRT signal back into ASR (SET/RESET) and AE (TOGGLE).
- **[ASRT_3AE_TO_SRT](ASRT_3AE_TO_SRT.md)**: the same merge, but with three individual AE sockets instead of a ready-made ASR adapter for SET/RESET.
- **[ASR_2AE_TO_SR](ASR_2AE_TO_SR.md)**: the `TOGGLE`-reduced variant for ASR.

## Conclusion

`ASRT_SR_AE_TO_SRT` is a simple, pure-wiring solution that extends an existing ASR adapter with a separate toggle event into a complete ASRT adapter.

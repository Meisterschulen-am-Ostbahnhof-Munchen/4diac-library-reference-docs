# ASRT_3AE_TO_SRT

![ASRT_3AE_TO_SRT](ASRT_3AE_TO_SRT.svg)

* * * * * * * * * *

## Introduction

The function block **ASRT_3AE_TO_SRT** is a composite FB that merges three separate unidirectional AE adapters (pure event, no payload data) into one common ASRT adapter (a set/reset/toggle event triple). It is the `TOGGLE`-extended variant of [ASR_2AE_TO_SR](ASR_2AE_TO_SR.md) and the adapter-based counterpart of [ASRT_3EVENTS_TO_SRT](ASRT_3EVENTS_TO_SRT.md).

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
| Socket | `SET_IN` | `adapter::types::unidirectional::AE` | Set / switch on. |
| Socket | `RESET_IN` | `adapter::types::unidirectional::AE` | Reset / switch off. |
| Socket | `TOGGLE_IN` | `adapter::types::unidirectional::AE` | Toggle / invert output. |
| Plug | `ASRT_OUT` | `adapter::types::unidirectional::ASRT` | Merged set/reset/toggle signal. |

## Functionality

The block is a pure FBNetwork of three event connections with no algorithms of its own: `SET_IN.E1` is wired directly to `ASRT_OUT.SET`, `RESET_IN.E1` to `ASRT_OUT.RESET`, and `TOGGLE_IN.E1` to `ASRT_OUT.TOGGLE`. An event at the respective AE socket immediately triggers the matching event at the ASRT plug.

## Technical Features

- **Pure wiring**: A composite FB with no ECC or algorithms, consisting solely of three direct event connections.
- **Adapter-based instead of event-based**: Unlike [ASRT_3EVENTS_TO_SRT](ASRT_3EVENTS_TO_SRT.md) (classic event inputs), this block uses AE adapter sockets, letting it fit seamlessly into adapter-based networks.

## State Overview

The function block has **no state machine**. Every event at `SET_IN`, `RESET_IN`, or `TOGGLE_IN` is forwarded immediately as the matching event type to `ASRT_OUT`.

## Application Scenarios

- **Merging three AE signal sources** (e.g. from [ASRT_MERGE_2](../../../events/unidirectional/EVENT/ASRT_MERGE_2.md) or directly from sensors) into a single ASRT adapter for downstream set/reset/toggle logic such as `AX_T_FF_SR`.
- **Adapterizing** existing AE-based networks to connect them to ASRT interfaces.

## Comparison with Similar Components

- **[ASRT_SRT_TO_3AE](ASRT_SRT_TO_3AE.md)**: the reverse direction – splits an ASRT signal back into three AE adapters.
- **[ASRT_3EVENTS_TO_SRT](ASRT_3EVENTS_TO_SRT.md)**: the same functionality with classic event inputs instead of AE adapter sockets.
- **[ASR_2AE_TO_SR](ASR_2AE_TO_SR.md)**: the `TOGGLE`-reduced variant for ASR.
- **[ASRT_SR_AE_TO_SRT](ASRT_SR_AE_TO_SRT.md)**: the same merge, but with `SET`/`RESET` already provided as a ready-made ASR adapter instead of two separate AE sockets.

## Conclusion

`ASRT_3AE_TO_SRT` is a simple, pure-wiring solution for merging three AE adapters into one ASRT adapter, suitable for seamlessly integrating AE-based event sources into ASRT interfaces.

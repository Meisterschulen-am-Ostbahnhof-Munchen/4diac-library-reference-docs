# ASRT_SRT_TO_3AE

![ASRT_SRT_TO_3AE](ASRT_SRT_TO_3AE.svg)

* * * * * * * * * *

## Introduction

The function block **ASRT_SRT_TO_3AE** is a composite FB that splits a unidirectional ASRT adapter (a set/reset/toggle event triple) into three separate AE adapters (pure event, no payload data). It is the reverse of [ASRT_3AE_TO_SRT](ASRT_3AE_TO_SRT.md) and the adapter-based counterpart of [ASRT_SRT_TO_3EVENTS](ASRT_SRT_TO_3EVENTS.md).

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
| Plug | `SET_OUT` | `adapter::types::unidirectional::AE` | Set / switch on. |
| Plug | `RESET_OUT` | `adapter::types::unidirectional::AE` | Reset / switch off. |
| Plug | `TOGGLE_OUT` | `adapter::types::unidirectional::AE` | Toggle / invert output. |

## Functionality

The block is a pure FBNetwork of three event connections with no algorithms of its own: `ASRT_IN.SET` is wired directly to `SET_OUT.E1`, `ASRT_IN.RESET` to `RESET_OUT.E1`, and `ASRT_IN.TOGGLE` to `TOGGLE_OUT.E1`. A SET, RESET, or TOGGLE event at the ASRT socket immediately triggers the matching event at the respective AE plug.

## Technical Features

- **Pure wiring**: A composite FB with no ECC or algorithms, consisting solely of three direct event connections.
- **Adapter-based instead of event-based**: Unlike [ASRT_SRT_TO_3EVENTS](ASRT_SRT_TO_3EVENTS.md) (classic event outputs), this block provides AE adapter plugs, letting it fit seamlessly into adapter-based networks.

## State Overview

The function block has **no state machine**. Every `SET`, `RESET`, or `TOGGLE` event at `ASRT_IN` is forwarded immediately to the respective AE plug.

## Application Scenarios

- **Fanning out an ASRT signal** (e.g. from `AX_T_FF_SR`) into three independently processable AE events.
- **Adapterizing** existing ASRT-based networks for further processing with plain AE adapters.

## Comparison with Similar Components

- **[ASRT_3AE_TO_SRT](ASRT_3AE_TO_SRT.md)**: the reverse direction – merges three AE adapters into one ASRT signal.
- **[ASRT_SRT_TO_3EVENTS](ASRT_SRT_TO_3EVENTS.md)**: the same functionality with classic event outputs instead of AE adapter plugs.
- **[ASR_SR_TO_2AE](ASR_SR_TO_2AE.md)**: the `TOGGLE`-reduced variant for ASR.
- **[ASRT_SRT_TO_SR_AE](ASRT_SRT_TO_SR_AE.md)**: the same split, but providing `SET`/`RESET` as a ready-made ASR adapter instead of two separate AE plugs.

## Conclusion

`ASRT_SRT_TO_3AE` is a simple, pure-wiring solution for splitting an ASRT adapter into three AE adapters, suitable for seamlessly integrating ASRT signals into AE-based networks.

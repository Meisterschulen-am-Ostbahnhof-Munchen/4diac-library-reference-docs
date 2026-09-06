# ASR_2AE_TO_SR

![ASR_2AE_TO_SR](ASR_2AE_TO_SR.svg)

* * * * * * * * * *

## Introduction

The function block **ASR_2AE_TO_SR** is a composite FB that merges two separate unidirectional AE adapters (pure event, no payload data) into one common ASR adapter (a set/reset event pair). It is the adapter-based counterpart of [ASR_2EVENTS_TO_SR](ASR_2EVENTS_TO_SR.md), but uses AE adapter sockets instead of classic event inputs.

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
| Plug | `ASR_OUT` | `adapter::types::unidirectional::ASR` | Merged set/reset signal. |

## Functionality

The block is a pure FBNetwork of two event connections with no algorithms of its own: `SET_IN.E1` is wired directly to `ASR_OUT.SET`, `RESET_IN.E1` directly to `ASR_OUT.RESET`. An event at the respective AE socket immediately triggers the matching SET or RESET event at the ASR plug.

## Technical Features

- **Pure wiring**: A composite FB with no ECC or algorithms, consisting solely of two direct event connections.
- **Adapter-based instead of event-based**: Unlike [ASR_2EVENTS_TO_SR](ASR_2EVENTS_TO_SR.md) (classic event inputs `SET`/`RESET`), this block uses AE adapter sockets, letting it fit seamlessly into adapter-based networks.

## State Overview

The function block has **no state machine**. Every event at `SET_IN` or `RESET_IN` is forwarded immediately as `SET` or `RESET` to `ASR_OUT`.

## Application Scenarios

- **Merging two AE signal sources** (e.g. from [ASR_MERGE_2](../../../events/unidirectional/EVENT/ASR_MERGE_2.md) or directly from a sensor) into a single ASR adapter for downstream set/reset logic.
- **Adapterizing** existing AE-based networks to connect them to ASR interfaces (e.g. `AX_SR`).

## Comparison with Similar Components

- **[ASR_SR_TO_2AE](ASR_SR_TO_2AE.md)**: the reverse direction – splits an ASR signal back into two AE adapters.
- **[ASR_2EVENTS_TO_SR](ASR_2EVENTS_TO_SR.md)**: the same functionality with classic event inputs instead of AE adapter sockets.
- **[ASRT_3AE_TO_SRT](ASRT_3AE_TO_SRT.md)**: the `TOGGLE`-extended variant for ASRT.

## Conclusion

`ASR_2AE_TO_SR` is a simple, pure-wiring solution for merging two AE adapters into one ASR adapter, suitable for seamlessly integrating AE-based event sources into ASR interfaces.

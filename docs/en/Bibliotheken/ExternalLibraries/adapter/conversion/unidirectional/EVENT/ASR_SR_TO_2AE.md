# ASR_SR_TO_2AE

![ASR_SR_TO_2AE](ASR_SR_TO_2AE.svg)

* * * * * * * * * *

## Introduction

The function block **ASR_SR_TO_2AE** is a composite FB that splits a unidirectional ASR adapter (a set/reset event pair) into two separate AE adapters (pure event, no payload data). It is the reverse of [ASR_2AE_TO_SR](ASR_2AE_TO_SR.md) and the adapter-based counterpart of [ASR_SR_TO_2EVENTS](ASR_SR_TO_2EVENTS.md).

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
| Socket | `ASR_IN` | `adapter::types::unidirectional::ASR` | Incoming set/reset signal. |
| Plug | `SET_OUT` | `adapter::types::unidirectional::AE` | Set / switch on. |
| Plug | `RESET_OUT` | `adapter::types::unidirectional::AE` | Reset / switch off. |

## Functionality

The block is a pure FBNetwork of two event connections with no algorithms of its own: `ASR_IN.SET` is wired directly to `SET_OUT.E1`, `ASR_IN.RESET` directly to `RESET_OUT.E1`. A SET or RESET event at the ASR socket immediately triggers the matching event at the respective AE plug.

## Technical Features

- **Pure wiring**: A composite FB with no ECC or algorithms, consisting solely of two direct event connections.
- **Adapter-based instead of event-based**: Unlike [ASR_SR_TO_2EVENTS](ASR_SR_TO_2EVENTS.md) (classic event outputs), this block provides AE adapter plugs, letting it fit seamlessly into adapter-based networks.

## State Overview

The function block has **no state machine**. Every `SET` or `RESET` event at `ASR_IN` is forwarded immediately to `SET_OUT` or `RESET_OUT`.

## Application Scenarios

- **Fanning out an ASR signal** into two independently processable AE events, e.g. to route SET and RESET separately to different downstream blocks.
- **Adapterizing** existing ASR-based networks for further processing with plain AE adapters.

## Comparison with Similar Components

- **[ASR_2AE_TO_SR](ASR_2AE_TO_SR.md)**: the reverse direction – merges two AE adapters into one ASR signal.
- **[ASR_SR_TO_2EVENTS](ASR_SR_TO_2EVENTS.md)**: the same functionality with classic event outputs instead of AE adapter plugs.
- **[ASRT_SRT_TO_3AE](ASRT_SRT_TO_3AE.md)**: the `TOGGLE`-extended variant for ASRT.

## Conclusion

`ASR_SR_TO_2AE` is a simple, pure-wiring solution for splitting an ASR adapter into two AE adapters, suitable for seamlessly integrating ASR signals into AE-based networks.

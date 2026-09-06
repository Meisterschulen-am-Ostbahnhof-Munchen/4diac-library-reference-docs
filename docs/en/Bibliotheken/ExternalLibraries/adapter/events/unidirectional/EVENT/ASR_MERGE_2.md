# ASR_MERGE_2

![ASR_MERGE_2](./ASR_MERGE_2.svg)

* * * * * * * * * *

## Introduction

The function block **ASR_MERGE_2** merges two unidirectional ASR adapters (a set/reset event pair, no payload data) into one common output adapter. It is the reverse of [ASR_SPLIT_2](ASR_SPLIT_2.md): instead of distributing a set/reset signal to two outputs, it merges two independent set/reset sources into a common output. The block is implemented as a generic FB (`GenericClassName: 'GEN_ASR_MERGE'`).

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
| Socket (Input 1) | `IN1` | `adapter::types::unidirectional::ASR` | First SET/RESET source. |
| Socket (Input 2) | `IN2` | `adapter::types::unidirectional::ASR` | Second SET/RESET source. |
| Plug (Output) | `OUT` | `adapter::types::unidirectional::ASR` | Merged SET/RESET signal. |

## Functionality

Whenever a `SET` event arrives at `IN1` **or** `IN2`, a `SET` is forwarded to `OUT`; likewise, a `RESET` at `IN1`/`IN2` is forwarded to `OUT` as a `RESET`. Both event types are handled independently – a `SET` at `IN1` has no effect on whether a `RESET` at `IN2` is forwarded. Since `ASR` carries no data value, there is nothing to arbitrate beyond the respective event type itself.

## Technical Features

- **Generic type**: Implemented via the generic base class `CGenUnidirectMergeBase` (N sockets, 1 plug) – the same C++ base underlying `GEN_AE_MERGE` and `GEN_ASRT_MERGE`.
- **Two independent event channels**: `SET` and `RESET` are checked and forwarded separately; one socket could, for example, only ever deliver `SET` events while the other only delivers `RESET`, without the two interfering with each other.
- **No states / algorithms**: Since `ASR` carries no data value, there is no change detection and no ECC.

## State Overview

The function block has **no state machine**. Its behavior is purely combinational: every incoming `SET` or `RESET` event at `IN1` or `IN2` is forwarded to `OUT` immediately as the matching event type.

## Application Scenarios

- **Merging redundant set/reset sources**: Two independent operator stations or safety paths should both be able to set or reset the same ASR output.
- **Simplifying networks** that would otherwise need two separate SET/RESET event connection pairs to the same destination.

## Comparison with Similar Components

- **[ASR_SPLIT_2](ASR_SPLIT_2.md)**: the reverse direction – distributes one incoming set/reset signal to two outputs, instead of merging two inputs.
- **[AE_MERGE_2](AE_MERGE_2.md)**: the same merge logic for the pure event adapter `AE` (1 event instead of SET/RESET).
- **[ASRT_MERGE_2](ASRT_MERGE_2.md)**: the same merge logic for the set/reset/toggle event adapter `ASRT` (adds `TOGGLE`).

## Conclusion

**ASR_MERGE_2** is a minimal generic function block for merging two set/reset event sources into a common ASR adapter output, closing the gap previously noted as "hypothetical" next to [ASR_SPLIT_2](ASR_SPLIT_2.md).

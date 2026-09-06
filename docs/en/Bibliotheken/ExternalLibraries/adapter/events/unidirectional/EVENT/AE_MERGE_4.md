# AE_MERGE_4

![AE_MERGE_4](AE_MERGE_4.svg)

* * * * * * * * * *

## Introduction

The function block **AE_MERGE_4** merges 4 unidirectional AE adapters (pure event adapter, no payload data) into one common output adapter. It is the 4-input variant of [AE_MERGE_2](AE_MERGE_2.md) and, like it, is implemented as a generic FB (`GenericClassName: 'GEN_AE_MERGE'`) — the same C++ base (`CGenUnidirectMergeBase`), just with 4 instead of 2 sockets.

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
| Socket (Input 1) | `IN1` | `adapter::types::unidirectional::AE` | Source 1. |
| Socket (Input 2) | `IN2` | `adapter::types::unidirectional::AE` | Source 2. |
| Socket (Input 3) | `IN3` | `adapter::types::unidirectional::AE` | Source 3. |
| Socket (Input 4) | `IN4` | `adapter::types::unidirectional::AE` | Source 4. |
| Plug (Output) | `OUT` | `adapter::types::unidirectional::AE` | Merged signal. |

## Functionality

Whenever `E1` arrives at any of the 4 sockets (`IN1`, `IN2`, `IN3`, `IN4`), it is forwarded unchanged to `OUT` as the matching event type. All event types are handled independently, and all 4 sockets are treated equally — the order in which sockets are checked follows `IN1` … `IN4`, which only matters if several events arrive within the same execution cycle. Since `AE` carries no data value, there is nothing to arbitrate beyond the respective event type itself.

## Technical Features

- **Generic type**: Implemented via the generic base class `CGenUnidirectMergeBase` (N sockets, 1 plug); the number of input sockets is fixed at compile time via the generic suffix (`_4`).
- **No states / algorithms**: Since `AE` carries no data value, there is no change detection and no ECC.
- **Arbitrary input count**: The same implementation covers AE_MERGE_2 through AE_MERGE_7; for other input counts see `AE_MERGE_2`, `AE_MERGE_3`, `AE_MERGE_5`, `AE_MERGE_6`, `AE_MERGE_7`.

## State Overview

The function block has **no state machine**. Its behavior is purely combinational: every incoming event at any of the 4 sockets is forwarded to `OUT` immediately.

## Application Scenarios

- **Merging several equivalent signal sources** (e.g. 4 redundant or alternative trigger paths) into a common AE adapter output.
- **Simplifying networks** that would otherwise need 4 separate event connections to the same destination.

## Comparison with Similar Components

- **[AE_SPLIT_2](AE_SPLIT_2.md)**: the reverse direction for 2 outputs (for ASRT, AE_SPLIT_3 through AE_SPLIT_9 are also available).
- **`AE_MERGE_2`, `AE_MERGE_3`, `AE_MERGE_5`, `AE_MERGE_6`, `AE_MERGE_7`**: the same generic implementation with a different input count.
- [ASR_MERGE_4](ASR_MERGE_4.md), [ASRT_MERGE_4](ASRT_MERGE_4.md): the same merge logic for the other unidirectional event adapters.

## Conclusion

`AE_MERGE_4` provides a generically implemented merge of 4 `AE` event sources into a common adapter output, completing the `GEN_AE_MERGE` family with the 4-input variant.

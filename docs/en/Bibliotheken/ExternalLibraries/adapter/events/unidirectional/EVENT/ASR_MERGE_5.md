# ASR_MERGE_5

![ASR_MERGE_5](ASR_MERGE_5.svg)

* * * * * * * * * *

## Introduction

The function block **ASR_MERGE_5** merges 5 unidirectional ASR adapters (set/reset event adapter, no payload data) into one common output adapter. It is the 5-input variant of [ASR_MERGE_2](ASR_MERGE_2.md) and, like it, is implemented as a generic FB (`GenericClassName: 'GEN_ASR_MERGE'`) — the same C++ base (`CGenUnidirectMergeBase`), just with 5 instead of 2 sockets.

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
| Socket (Input 1) | `IN1` | `adapter::types::unidirectional::ASR` | Source 1. |
| Socket (Input 2) | `IN2` | `adapter::types::unidirectional::ASR` | Source 2. |
| Socket (Input 3) | `IN3` | `adapter::types::unidirectional::ASR` | Source 3. |
| Socket (Input 4) | `IN4` | `adapter::types::unidirectional::ASR` | Source 4. |
| Socket (Input 5) | `IN5` | `adapter::types::unidirectional::ASR` | Source 5. |
| Plug (Output) | `OUT` | `adapter::types::unidirectional::ASR` | Merged signal. |

## Functionality

Whenever `SET` or `RESET` arrives at any of the 5 sockets (`IN1`, `IN2`, `IN3`, `IN4`, `IN5`), it is forwarded unchanged to `OUT` as the matching event type. All event types are handled independently, and all 5 sockets are treated equally — the order in which sockets are checked follows `IN1` … `IN5`, which only matters if several events arrive within the same execution cycle. Since `ASR` carries no data value, there is nothing to arbitrate beyond the respective event type itself.

## Technical Features

- **Generic type**: Implemented via the generic base class `CGenUnidirectMergeBase` (N sockets, 1 plug); the number of input sockets is fixed at compile time via the generic suffix (`_5`).
- **No states / algorithms**: Since `ASR` carries no data value, there is no change detection and no ECC.
- **Arbitrary input count**: The same implementation covers ASR_MERGE_2 through ASR_MERGE_7; for other input counts see `ASR_MERGE_2`, `ASR_MERGE_3`, `ASR_MERGE_4`, `ASR_MERGE_6`, `ASR_MERGE_7`.

## State Overview

The function block has **no state machine**. Its behavior is purely combinational: every incoming event at any of the 5 sockets is forwarded to `OUT` immediately.

## Application Scenarios

- **Merging several equivalent signal sources** (e.g. 5 redundant or alternative trigger paths) into a common ASR adapter output.
- **Simplifying networks** that would otherwise need 5 separate event connections to the same destination.

## Comparison with Similar Components

- **[ASR_SPLIT_2](ASR_SPLIT_2.md)**: the reverse direction for 2 outputs (for ASRT, ASR_SPLIT_3 through ASR_SPLIT_9 are also available).
- **`ASR_MERGE_2`, `ASR_MERGE_3`, `ASR_MERGE_4`, `ASR_MERGE_6`, `ASR_MERGE_7`**: the same generic implementation with a different input count.
- [AE_MERGE_5](AE_MERGE_5.md), [ASRT_MERGE_5](ASRT_MERGE_5.md): the same merge logic for the other unidirectional event adapters.

## Conclusion

`ASR_MERGE_5` provides a generically implemented merge of 5 `ASR` event sources into a common adapter output, completing the `GEN_ASR_MERGE` family with the 5-input variant.

# A2X_AUI_MUX_2_UNGATED

![A2X_AUI_MUX_2_UNGATED](./A2X_AUI_MUX_2_UNGATED.svg)

* * * * * * * * * *

## Introduction

A2X_AUI_MUX_2_UNGATED is an adapter-based, event-driven multiplexer for selecting one of two unidirectional A2X data streams and forwarding it to a single output. It is an ungated variant of the A2X_AUI_MUX_2 function block. In contrast to a gated multiplexer, it does not perform any change detection. Every newly computed result is forwarded unconditionally, regardless of whether the value has actually changed.

This behavior is intended for consumers that require a periodic cadence or a regular data flow, such as derivative or frequency calculations, where the arrival of each new result is important even if the value remains identical to the previous one.

## Interface Structure

The function block has no conventional event inputs or data inputs/outputs. Its interface is entirely built around unidirectional adapters. The only direct event output is a confirmation event.

### **Event Inputs**

None.

### **Event Outputs**

| Event | Description |
|-------|-------------|
| `CNF` | Confirmation that the index selection requested via the `K` adapter has been applied. |

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

| Kind | Name | Type | Description |
|------|------|------|-------------|
| Socket | `K`   | `adapter::types::unidirectional::AUI` | Selection index. Selects `IN1` when `K = 0` and `IN2` when `K = 1`. |
| Socket | `IN1` | `adapter::types::unidirectional::A2X` | First value stream, selected when `K = 0`. |
| Socket | `IN2` | `adapter::types::unidirectional::A2X` | Second value stream, selected when `K = 1`. |
| Plug   | `OUT` | `adapter::types::unidirectional::A2X` | Multiplexed output stream. |

## Functionality

The block acts as a two-input multiplexer for unidirectional adapter streams. The selection index is received through the `AUI` adapter socket `K`. When `K = 0`, the data arriving via `IN1` is passed to `OUT`. When `K = 1`, the data arriving via `IN2` is passed to `OUT`.

The "UNGATED" characteristic means that the block does not compare incoming values with previous values. Every incoming result is forwarded immediately to the output. This makes the block well suited for applications in which downstream blocks need each processing cycle to be triggered, even if the underlying value has not changed.

The `CNF` event output provides a confirmation that the index set on `K` has been accepted and that the corresponding input stream is now routed to the output.

The block is a generic function block realized through the generic class name `GEN_A2X_AUI_MUX`. It is intended for use in unidirectional, adapter-based selection scenarios.

## Technical Features

- Adapter-based interface with no direct data inputs or outputs.
- Supports one `AUI` index socket for selecting one of two `A2X` input sockets.
- Provides a single unidirectional `A2X` output plug.
- Forwards all incoming results unconditionally without change detection.
- Emits a `CNF` confirmation event after applying the index.
- Generic implementation using `GEN_A2X_AUI_MUX`.
- Package: `adapter::selection::unidirectional`.
- Suitable for periodic and cadence-sensitive data flow architectures.

## State Overview

The block does not expose an explicit state machine. It behaves as a combinatorial, event-triggered adapter multiplexer. In the ungated variant, no internal state is used to store or compare previous input values. This means there is no hysteresis, no change detection latch, and no suppression of repeated values.

## Application Scenarios

- **Derivative/Frequency Calculations**: Downstream blocks that need a new result at every input event, regardless of whether the value changed, benefit from the unconditional forwarding behavior.
- **Periodic Data Distribution**: When a consumer requires a fixed periodic data cadence, the ungated multiplexer ensures that every computed result is delivered.
- **Adapter-Based Selecting**: When two unidirectional data sources must be selected dynamically through an adapter-based index, this block provides a clean and reusable solution.
- **Stream Redirection**: Useful in processing chains where the active input source may switch between two producers without losing event rhythm.

## Comparison with Similar Blocks

- **A2X_AUI_MUX_2**: The gated variant performs change detection. It only forwards data when the incoming value differs from the previously forwarded value. This reduces unnecessary downstream activity but can break a periodic cadence.
- **A2X_AUI_MUX_2_UNGATED**: This variant forwards every newly calculated result without compare-and-skip logic. It preserves the event rhythm but may produce repeated identical values on the output.
- **Conventional MUX blocks**: Standard multiplexers typically use direct data inputs and a scalar selector. This block instead uses unidirectional adapters, making it more suitable for structured, event-driven adapter communication.

## Conclusion

A2X_AUI_MUX_2_UNGATED is a compact, adapter-based multiplexer that selects between two unidirectional A2X streams using an AUI index. Its key feature is the absence of change detection: every result is forwarded immediately. This makes it ideal for applications that rely on a stable periodic event flow, such as derivative or frequency calculations, where the arrival of each result itself carries information.

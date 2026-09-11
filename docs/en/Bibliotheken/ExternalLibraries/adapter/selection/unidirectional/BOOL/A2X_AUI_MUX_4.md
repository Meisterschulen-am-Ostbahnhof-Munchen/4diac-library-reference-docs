# A2X_AUI_MUX_4

![A2X_AUI_MUX_4](./A2X_AUI_MUX_4.svg)

* * * * * * * * * *

## Introduction

A2X_AUI_MUX_4 is a generic, adapter-based 4-to-1 multiplexer for Eclipse 4diac. It connects four unidirectional A2X adapter inputs (`IN1` ... `IN4`) to one A2X adapter output (`OUT`). The active input is selected by the value received on the AUI adapter `K`. The output is not simply forwarded from the selected input; it is updated only when the selected value actually changes. The `CNF` event is emitted only in this case, avoiding unnecessary downstream events.

## Interface Structure

The FB has no ordinary event or data inputs/outputs. All control and data information is exchanged through adapter interfaces. Only one event output, `CNF`, is present.

### **Event Inputs**

None.

### **Event Outputs**

| Name | Type | Description |
|------|------|-------------|
| `CNF` | Event | Confirmation that the selected index has been processed and `OUT` has been updated. Due to change detection, `CNF` is only emitted when the selected value actually changes. |

### **Data Inputs**

None. All payload data is supplied via adapter sockets.

### **Data Outputs**

None. All payload data is provided through the `OUT` adapter plug.

### **Adapters**

| Port | Name | Type | Direction | Description |
|------|------|------|-----------|-------------|
| Socket | `K` | `adapter::types::unidirectional::AUI` | Input | Carries the selection index. |
| Socket | `IN1` | `adapter::types::unidirectional::A2X` | Input | Value selected when K = 0. |
| Socket | `IN2` | `adapter::types::unidirectional::A2X` | Input | Value selected when K = 1. |
| Socket | `IN3` | `adapter::types::unidirectional::A2X` | Input | Value selected when K = 2. |
| Socket | `IN4` | `adapter::types::unidirectional::A2X` | Input | Value selected when K = 3. |
| Plug | `OUT` | `adapter::types::unidirectional::A2X` | Output | Selected value; updated only when the selected value changes. |

## Functionality

The block continuously receives the selector value `K` and the four A2X input adapter values. It determines which input is active according to the current value of `K`:

- K = 0 selects `IN1`.
- K = 1 selects `IN2`.
- K = 2 selects `IN3`.
- K = 3 selects `IN4`.

When the active index changes, or when the value on the currently active input changes, the block compares the new selected value with the value previously sent to `OUT`. If the values differ, `OUT` is updated with the new value and the `CNF` event is emitted. If the values are equal, the output remains unchanged and no event is generated. This prevents event flooding and ensures that downstream adapter consumers only react to real value changes.

## Technical Features

- Generic FB implementation: the `eclipse4diac::core::GenericClassName` attribute is `GEN_A2X_AUI_MUX`, so the concrete behavior is provided by a generic runtime backend with this name.
- Adapter-based interface: uses unidirectional `A2X` and `AUI` adapter types; no elementary data ports are required.
- Change-driven output: `OUT` is only updated on actual selected-value changes.
- Change-driven confirmation: `CNF` is emitted only when an actual change occurs.
- Four selectable inputs: enough for 4-to-1 selection scenarios.
- Compiler/package namespace: `adapter::selection::unidirectional`.

## State Overview

The XML definition does not contain an explicit ECC state machine; the actual behavior is implemented by the generic backend `GEN_A2X_AUI_MUX`. Conceptually, the block maintains a single internal state: the last value forwarded to `OUT`. The relevant state transition is:

- **No change**: selected value equals the last forwarded value → no output update, no `CNF` event.
- **Change**: selected value differs from the last forwarded value → `OUT` is updated, `CNF` event is emitted.

This implicit state allows the block to suppress repeated transmissions of identical values.

## Application Scenarios

- Selecting one of four A2X data sources at runtime, using an AUI adapter as the selector.
- Connecting multiple A2X producer adapters to a single consumer without exposing elementary data types.
- Event-driven systems where downstream components should be triggered only when the selected value actually changes.
- Reducing unnecessary network or adapter communication in distributed 4diac applications.
- Using as an adapter-based alternative to a classic multiplexer in Eclipse 4diac.

## Comparison with Similar Blocks

A2X_AUI_MUX_4 is an A2X variant of AX_AUI_MUX_4. Compared with a conventional data-input multiplexer, it uses adapter ports instead of elementary data inputs and outputs. This makes it suitable for structured or adapter-based data exchange.

| Block / Approach | Interface | Update behavior |
|------------------|-----------|-----------------|
| A2X_AUI_MUX_4 | 4 × A2X sockets, 1 × AUI socket, 1 × A2X plug | Output and `CNF` event only on actual value change. |
| AX_AUI_MUX_4 | Adapter-based, non-A2X variant | Similar selection structure; different adapter type. |
| Classic MUX | Elementary data inputs and data output | Usually event-triggered; may forward unchanged values and emit an event on every selection. |

The change-detection behavior of A2X_AUI_MUX_4 is its main distinction from a standard multiplexer.

## Conclusion

A2X_AUI_MUX_4 provides an adapter-oriented, change-driven multiplexing solution for Eclipse 4diac. It selects one of four A2X inputs using an AUI selector and forwards the selected value through an A2X output. Because the output and the confirmation event are suppressed when no actual value change occurs, the block helps keep event chains lean and efficient. It is especially useful in adapter-based automation architectures where only meaningful data changes should propagate to consumers.

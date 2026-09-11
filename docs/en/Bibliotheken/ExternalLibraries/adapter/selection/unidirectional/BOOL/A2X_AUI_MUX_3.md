# A2X_AUI_MUX_3

![A2X_AUI_MUX_3](./A2X_AUI_MUX_3.svg)

* * * * * * * * * *

## Introduction

A2X_AUI_MUX_3 is a generic adapter-based 3-to-1 multiplexer function block for IEC 61499 applications. It selects one of three A2X input adapters and forwards the selected value to an A2X output adapter. The selection index is supplied through an AUI adapter. The block is implemented as the typed generic variant `GEN_A2X_AUI_MUX` and is designed so that the output adapter is only updated when the selected value actually changes. The confirmation event `CNF` is therefore emitted only after a real value change, not for redundant index updates.

## Interface Structure

The function block has no direct data inputs or data outputs. All value and control information is exchanged through the adapter interface.

### **Event Inputs**

None.

### **Event Outputs**

| Name | Type | Comment |
|------|------|---------|
| `CNF` | Event | Confirmation of set index K |

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

| Direction | Name | Type | Description |
|-----------|------|------|-------------|
| Plug | `OUT` | `adapter::types::unidirectional::A2X` | Selected input value: IN1 for K = 0, IN2 for K = 1, IN3 for K = 2 |
| Socket | `K` | `adapter::types::unidirectional::AUI` | Index used to select the active multiplexer input |
| Socket | `IN1` | `adapter::types::unidirectional::A2X` | Input value 1, selected when K = 0 |
| Socket | `IN2` | `adapter::types::unidirectional::A2X` | Input value 2, selected when K = 1 |
| Socket | `IN3` | `adapter::types::unidirectional::A2X` | Input value 3, selected when K = 2 |

## Functionality

A2X_AUI_MUX_3 behaves as an adapter-based multiplexer:

1. A new selection index is received through the `K` adapter.
2. The index value determines which input adapter is active:
   - K = 0 selects `IN1`
   - K = 1 selects `IN2`
   - K = 2 selects `IN3`
3. The value of the selected input adapter is compared with the current value on the output adapter `OUT`.
4. If the value has changed, the output adapter is updated and the `CNF` event is emitted.
5. If the value has not changed, no output update and no `CNF` event occur.

This behavior reduces unnecessary event and data traffic on the adapter connection. The block is therefore especially useful in applications where the index may be set repeatedly to the same value, but only actual value changes should be propagated.

## Technical Features

- Generic implementation based on `GEN_A2X_AUI_MUX`
- Three A2X input adapters: `IN1`, `IN2`, `IN3`
- One A2X output adapter: `OUT`
- One AUI index adapter: `K`
- Fully adapter-based interface; no direct data inputs or outputs
- Unidirectional adapter communication
- Change-only output update behavior
- Confirmation event `CNF` emitted only on actual value change
- Suitable for distributed IEC 61499 applications
- Licensed under EPL-2.0

## State Overview

The block does not declare an explicit state machine in its interface definition. Its observable behavior can be described by the following conceptual states:

- **Idle**: waiting for a new index value on the `K` adapter.
- **Evaluate**: reading the index and selecting the corresponding input adapter.
- **Compare**: checking whether the selected input value differs from the current output value.
- **Publish**: updating the `OUT` adapter and emitting `CNF` if an actual change occurred.

This state behavior ensures that redundant selections do not cause unnecessary output activity.

## Application Scenarios

A2X_AUI_MUX_3 is suitable for the following use cases:

- Selecting one of three process values or measurement signals connected via adapters.
- Routing different data sources to a single consumer without using direct data connections.
- Building adapter-based signal selection logic in distributed control systems.
- Reducing network or communication load by suppressing confirmation events when the selected value is unchanged.
- Using as a reusable building block in larger adapter-based multiplexer or routing structures.

## Comparison with Similar Blocks

Compared to a conventional data multiplexer, A2X_AUI_MUX_3 does not expose physical data inputs and outputs. Instead, it uses unidirectional adapters for both values and index, making integration in adapter-based system architectures easier.

In contrast to standard multiplexers that always copy the selected input to the output, A2X_AUI_MUX_3 only updates the output when the selected value actually changes. This makes it more efficient in event-driven systems.

The block is an A2X variant of the existing `AX_AUI_MUX_3` concept. The main difference is the use of the `A2X` adapter type for the value interfaces while the index remains on the `AUI` adapter type.

## Conclusion

A2X_AUI_MUX_3 is a compact, adapter-based 3-to-1 multiplexer that combines selection logic with change detection. Its unidirectional adapter interface and event-on-change behavior make it well suited for distributed IEC 61499 applications where efficient communication and minimal unnecessary events are important.

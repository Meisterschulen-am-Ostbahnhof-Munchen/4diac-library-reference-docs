# A2X_AUI_DEMUX_2_UNGATED

![A2X_AUI_DEMUX_2_UNGATED](./A2X_AUI_DEMUX_2_UNGATED.svg)

* * * * * * * * * *

## Introduction

A2X_AUI_DEMUX_2_UNGATED is an adapter-based generic demultiplexer function block. It routes an incoming value received through the `IN` adapter to one of two output adapters, `OUT1` or `OUT2`, depending on the index received through the `K` adapter.

The “UNGATED” variant does not perform any change detection. Every newly computed value or result is forwarded unconditionally. This makes the block suitable for consumers that require a periodic data cadence, independent of whether the actual payload value has changed, for example in derivative or frequency calculations.

## Interface Structure

The block does not use dedicated data inputs or data outputs. All value and index information is exchanged through unidirectional adapters. The only event interface element is the confirmation event `CNF`.

### **Event Inputs**

None. Incoming triggers are received through the adapter sockets.

### **Event Outputs**

| Name | Type   | Description                                |
|------|--------|--------------------------------------------|
| `CNF` | Event  | Confirmation that the index `K` has been set/applied. |

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

| Direction | Name   | Type                                             | Description                              |
|-----------|--------|--------------------------------------------------|------------------------------------------|
| Socket    | `K`    | `adapter::types::unidirectional::AUI`            | Index input used for output selection.   |
| Socket    | `IN`   | `adapter::types::unidirectional::A2X`            | Input value to be demultiplexed.         |
| Plug      | `OUT1` | `adapter::types::unidirectional::A2X`            | Output value 1, selected when `K = 0`.   |
| Plug      | `OUT2` | `adapter::types::unidirectional::A2X`            | Output value 2, selected when `K = 1`.   |

## Functionality

The function block operates as a one-to-two demultiplexer:

1. The index adapter `K` provides the selection information.
2. The value adapter `IN` provides the value to be routed.
3. If `K = 0`, the incoming value is forwarded to `OUT1`.
4. If `K = 1`, the incoming value is forwarded to `OUT2`.
5. After the index has been applied, the `CNF` event is issued as confirmation.

Because the block is “ungated”, it does not compare the current value against the previous value. Every new calculation result or incoming value is passed through even if the payload is unchanged. This behavior is essential for consumers that rely on a steady event stream or periodic update rate.

## Technical Features

- Adapter-based interface without separate data inputs or outputs.
- Unidirectional adapter types are used for both index and data transfer.
- Generic FB implementation via the attribute `eclipse4diac::core::GenericClassName` with value `GEN_A2X_AUI_DEMUX`.
- One event output: `CNF`.
- Two socket adapters: `K` and `IN`.
- Two plug adapters: `OUT1` and `OUT2`.
- No change detection or value gating logic.
- Designed for periodic, time-driven consumers.

## State Overview

The XML definition does not contain an explicit ECC state machine. The behavior is handled by the generic runtime implementation behind `GEN_A2X_AUI_DEMUX`.

The relevant runtime state is limited to:

- The currently selected output path, determined by the index `K`.
- The forwarding state of the input value `IN`.

Since the block is ungated, there is no state representing “value changed” or “value unchanged”. Every incoming processing cycle leads to unconditional forwarding to the selected output.

## Application Scenarios

A2X_AUI_DEMUX_2_UNGATED is useful in all cases where an incoming data stream must be distributed to one of two consumers based on an index, and where every computed result must be delivered.

Typical examples:

- Derivative or slope calculations that need to process every sample.
- Frequency or period measurement functions that require a fixed event cadence.
- Adapter-based unidirectional data distribution in a 4diac IEC 61499 application.
- Splitting a continuous value stream into two downstream processing branches.

## Comparison with Similar Blocks

The closest comparable block is `A2X_AUI_DEMUX_2`. Both blocks share the same adapter interface and output selection behavior.

The key difference is:

- `A2X_AUI_DEMUX_2` includes change detection and only forwards values when the result has changed.
- `A2X_AUI_DEMUX_2_UNGATED` disables this detection and forwards every newly computed result unconditionally.

This makes the ungated variant preferable for consumers that need a periodic trigger even when the data value is constant, while the gated variant is better suited for event-driven consumers that only want to react to actual value changes.

## Conclusion

A2X_AUI_DEMUX_2_UNGATED is a flexible, adapter-based generic demultiplexer for unidirectional data flow. It selects between two outputs using an index adapter and forwards every incoming value without change detection. This unconditional forwarding behavior makes it especially suitable for calculation chains that require a constant update cadence, such as derivative and frequency computations.

# A2X_AUI_DEMUX_3_UNGATED

![A2X_AUI_DEMUX_3_UNGATED](./A2X_AUI_DEMUX_3_UNGATED.svg)

* * * * * * * * * *

## Introduction

A2X_AUI_DEMUX_3_UNGATED is an adapter-based demultiplexer FB that distributes an incoming value stream from one input adapter to one of three output adapters. The selection is made through an index adapter. In contrast to gated demultiplexers, this FB does not perform any change detection: every newly computed result is forwarded unconditionally. It is therefore suitable for consumers that require a periodic data cadence even when the value itself has not changed, for example derivative or frequency calculations.

The FB uses the unidirectional A2X adapter type for data transport and a unidirectional AUI adapter for the index selection. It is declared as a generic FB with the generic class name `GEN_A2X_AUI_DEMUX`.

## Interface Structure

### **Event Inputs**

No event inputs are declared. The FB is driven entirely through its adapter sockets.

### **Event Outputs**

| Event | Description |
|---|---|
| `CNF` | Confirmation of Set Index K. This event signals that the index selection and forwarding step has been processed. |

### **Data Inputs**

No data inputs are declared.

### **Data Outputs**

No data outputs are declared.

### **Adapters**

| Direction | Adapter | Type | Description |
|---|---|---|---|
| Socket | `K` | `adapter::types::unidirectional::AUI` | Index selector. Determines which output is used. |
| Socket | `IN` | `adapter::types::unidirectional::A2X` | Input value stream to be demultiplexed. |
| Plug | `OUT1` | `adapter::types::unidirectional::A2X` | Output value 1, selected when `K = 0`. |
| Plug | `OUT2` | `adapter::types::unidirectional::A2X` | Output value 2, selected when `K = 1`. |
| Plug | `OUT3` | `adapter::types::unidirectional::A2X` | Output value 3, selected when `K = 2`. |

## Functionality

A2X_AUI_DEMUX_3_UNGATED forwards data received on the `IN` adapter to exactly one of the three output adapters. The output is chosen according to the index received on the `K` adapter:

- `K = 0` → `OUT1`
- `K = 1` → `OUT2`
- `K = 2` → `OUT3`

Each time a new value arrives on `IN`, the FB passes that value to the currently selected output. No comparison with a previously forwarded value is performed. This is the key difference between this block and a gated demultiplexer: the output is updated as often as new results arrive, regardless of whether the data content has changed.

After the forwarding step is completed, the event output `CNF` is emitted as confirmation. Because all data and selection information is transported through adapter connections, no separate input events or data inputs are required.

## Technical Features

- Three unidirectional output plugs: `OUT1`, `OUT2`, `OUT3`.
- One unidirectional input socket `IN` for the data stream to be demultiplexed.
- One unidirectional index socket `K` for output selection.
- Event output `CNF` for confirmation of the index selection.
- No change detection or gating logic.
- No internal storage of the last forwarded value.
- Generic FB implementation using the class name `GEN_A2X_AUI_DEMUX`.
- Intended for use in adapter-based selection and distribution scenarios.
- The adapter package is `adapter::selection::unidirectional`.

## State Overview

The FB does not define a complex state machine. Because no change detection is required, there is no state that stores and compares previous values. The operational behavior can be summarized as:

- Awaiting index selection through `K`.
- Awaiting a new value on `IN`.
- Forwarding the received value to the output selected by `K`.
- Emitting `CNF` to confirm the action.

This simple structure makes the FB deterministic and easy to use in periodic processing chains.

## Application Scenarios

A2X_AUI_DEMUX_3_UNGATED is useful wherever a data source must be routed to one of several consumers on every computation cycle. Typical scenarios include:

- Periodic derivative or frequency calculation where the consumer needs a fixed update rate even if the source value remains constant.
- Distribution of a process value to different functional modules depending on an operating mode.
- Adapter-based stream switching in automation applications.
- Demultiplexing of a single data stream into three alternative processing paths without requiring the source to detect value changes.

Because the block is un-gated, it is especially suitable for time-driven applications where the timing of the output event is more important than the value difference between two consecutive samples.

## Comparison with Similar Blocks

Compared with `A2X_AUI_DEMUX_3`, the `_UNGATED` variant differs in its forwarding behavior. The gated variant only forwards a value when the input value has changed. The un-gated variant forwards every newly computed value unconditionally. This makes `A2X_AUI_DEMUX_3_UNGATED` the preferred choice for consumers that need a continuous periodic stream of events.

Compared with the `AX_AUI_DEMUX_3_UNGATED` variant, this FB uses the `A2X` adapter type instead of `AX`. It is therefore suitable for systems built on the newer or different `A2X` unidirectional adapter family while preserving the same demultiplexing behavior.

## Conclusion

A2X_AUI_DEMUX_3_UNGATED is a simple and flexible adapter-based demultiplexer for three output paths. Its main advantage is the unconditional forwarding of every incoming value, which guarantees a regular output cadence. The use of adapter sockets for both index selection and data input makes it well suited for modern adapter-oriented IEC 61499 applications, especially where periodic downstream processing is required.

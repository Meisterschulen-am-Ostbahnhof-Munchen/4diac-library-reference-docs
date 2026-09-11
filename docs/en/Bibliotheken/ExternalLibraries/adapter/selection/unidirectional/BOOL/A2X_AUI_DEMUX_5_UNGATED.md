# A2X_AUI_DEMUX_5_UNGATED

![A2X_AUI_DEMUX_5_UNGATED](./A2X_AUI_DEMUX_5_UNGATED.svg)

* * * * * * * * * *

## Introduction

A2X_AUI_DEMUX_5_UNGATED is a unidirectional 1-to-5 demultiplexer FB. It forwards one input value received through the `A2X` adapter socket to one of five `A2X` output adapters, depending on an index value received through the `AUI` adapter socket.

In contrast to a gated demultiplexer, this block does **not** perform change detection. Every newly computed or received result is forwarded unconditionally. This makes the FB suitable for consumers that require a continuous periodic data cadence, such as derivative or frequency calculations, even when the value itself has not changed.

## Interface Structure

### **Event Inputs**

There are no explicit event inputs declared in the interface list. The FB is controlled through its adapter sockets and plugs.

### **Event Outputs**

| Event | Comment |
|-------|---------|
| `CNF` | Confirmation of Set Index K |

### **Data Inputs**

There are no explicit data inputs. The input value is transmitted through the adapter socket `IN`.

### **Data Outputs**

There are no explicit data outputs. The demultiplexed results are transmitted through the adapter plugs `OUT1` to `OUT5`.

### **Adapters**

| Adapter | Type | Direction | Description |
|---------|------|-----------|-------------|
| `K` | `adapter::types::unidirectional::AUI` | Socket | Index input used to select the active output |
| `IN` | `adapter::types::unidirectional::A2X` | Socket | Input value to demultiplex |
| `OUT1` | `adapter::types::unidirectional::A2X` | Plug | Output value 1, selected when `K = 0` |
| `OUT2` | `adapter::types::unidirectional::A2X` | Plug | Output value 2, selected when `K = 1` |
| `OUT3` | `adapter::types::unidirectional::A2X` | Plug | Output value 3, selected when `K = 2` |
| `OUT4` | `adapter::types::unidirectional::A2X` | Plug | Output value 4, selected when `K = 3` |
| `OUT5` | `adapter::types::unidirectional::A2X` | Plug | Output value 5, selected when `K = 4` |

## Functionality

The FB acts as a demultiplexer between one input adapter and five output adapters. The index supplied through the `K` adapter determines which output receives the current value from `IN`.

The `K` index mapping is fixed:

- `K = 0` → `OUT1`
- `K = 1` → `OUT2`
- `K = 2` → `OUT3`
- `K = 3` → `OUT4`
- `K = 4` → `OUT5`

The designation "UNGATED" indicates that no gating mechanism suppresses repeated values. If a new result arrives, it is forwarded to the selected output regardless of whether the value differs from the previous one. The `CNF` event output confirms that the index selection has been processed.

## Technical Features

- 1-to-5 unidirectional adapter-based demultiplexing.
- Uses `adapter::types::unidirectional::A2X` for the data path.
- Uses `adapter::types::unidirectional::AUI` for index selection.
- No value-change detection.
- Every delivered result is passed through unconditionally.
- Interface contract is based on a generic implementation class `GEN_A2X_AUI_DEMUX`.
- Suitable for periodic and time-triggered processing chains.
- Licensed under EPL-2.0.

## State Overview

This block does not rely on input value history to make forwarding decisions. Because it is ungated, it does not need to remember the last output value in order to suppress duplicate updates.

The only relevant runtime information is the currently selected index `K`. The observable behavior is therefore essentially stateless with respect to the incoming data values.

## Application Scenarios

- Periodic derivative or frequency calculations where a constant input value must still produce a steady output cadence.
- Distribution of one input stream to multiple alternative consumers.
- Test and simulation environments that need deterministic forwarding on every cycle.
- Systems where downstream blocks expect a continuous update signal even when the data has not changed.
- Generic adapter-based selection between five unidirectional output paths.

## Comparison with Similar Blocks

The gated variant `A2X_AUI_DEMUX_5` forwards a value only when a change is detected. `A2X_AUI_DEMUX_5_UNGATED` removes this restriction and forwards every newly produced result unconditionally.

This makes the gated variant useful for state-oriented or event-driven consumers that only react to actual changes. The ungated variant is better suited for time-driven calculations, control loops, and signal-processing blocks that require a regular data rhythm.

## Conclusion

`A2X_AUI_DEMUX_5_UNGATED` is a simple and predictable adapter-based demultiplexer. It provides a clean mapping from one input adapter to five output adapters using an external index. By omitting change detection, it guarantees that every result cycle is propagated to the selected output. This behavior is ideal for applications that require a constant update flow or periodicity, independent of whether the actual data value has changed.

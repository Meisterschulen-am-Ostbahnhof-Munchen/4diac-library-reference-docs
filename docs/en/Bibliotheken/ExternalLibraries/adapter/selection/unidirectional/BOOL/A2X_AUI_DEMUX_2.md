# A2X_AUI_DEMUX_2

![A2X_AUI_DEMUX_2](./A2X_AUI_DEMUX_2.svg)

* * * * * * * * * *

## Introduction

A2X_AUI_DEMUX_2 is a generic, unidirectional demultiplexer function block for 4diac-ide. It receives one value through an A2X adapter socket and forwards it to one of two A2X output adapters. The selected output is controlled by an index value received through the AUI adapter socket K.

The block is designed as a generic FB: the actual behavior is provided by the generic implementation class `GEN_A2X_AUI_DEMUX`. The adapter outputs are only updated when the routed value actually changes, and the confirmation event `CNF` is only emitted in that case.

## Interface Structure

The interface is adapter-centric. There are no direct data inputs or data outputs. All values are transported via the unidirectional adapter sockets and plugs.

### **Event Inputs**

There are no top-level event inputs. The FB is activated through data changes arriving on its adapter sockets.

### **Event Outputs**

| Event | Description |
|-------|-------------|
| `CNF` | Confirmation that the index K has been set and the selected output was updated. It is only emitted if an actual value change occurred. |

### **Data Inputs**

There are no direct data inputs. The input value is supplied through the `IN` adapter socket.

### **Data Outputs**

There are no direct data outputs. The demultiplexed values are provided through the `OUT1` and `OUT2` adapter plugs.

### **Adapters**

| Adapter | Direction | Type | Description |
|---------|-----------|------|-------------|
| `OUT1` | Plug / output | `A2X` | Output value 1, selected when K = 0 |
| `OUT2` | Plug / output | `A2X` | Output value 2, selected when K = 1 |
| `K` | Socket / input | `AUI` | Index input used to select the active output |
| `IN` | Socket / input | `A2X` | Input value to be demultiplexed |

## Functionality

The FB behaves as a 1-to-2 demultiplexer:

1. It receives an index value on the `K` socket.
2. If `K = 0`, the value from `IN` is routed to `OUT1`.
3. If `K = 1`, the value from `IN` is routed to `OUT2`.
4. The selected output adapter is updated only when the new value differs from the previously output value.
5. If a value is actually changed, the `CNF` event is emitted.
6. If the value is unchanged, no output update and no event take place.

This behavior reduces unnecessary communication between adapters and prevents downstream function blocks from reacting to unchanged values.

## Technical Features

- Generic FB with generic class name `GEN_A2X_AUI_DEMUX`.
- Package: `adapter::selection::unidirectional`.
- Fully adapter-based interface without direct data inputs or outputs.
- Unidirectional A2X and AUI adapter types.
- Two output adapters allow selecting between two destinations.
- Event output `CNF` for acknowledgment.
- Event generation is suppressed when the output value has not changed.
- Suitable for use inside modular IEC 61499 applications with adapter-based communication.

## State Overview

The XML type definition does not contain an explicit ECC state machine. The behavior is implemented by the generic backend. Conceptually, the FB can be viewed in the following states:

| State | Description |
|-------|-------------|
| `WAITING` | The FB waits for a new index value on `K` or a new input value on `IN`. |
| `ROUTING` | The current input value is routed to the output selected by `K`. |
| `UPDATING` | The selected output adapter is updated because the value has changed. |
| `CONFIRMING` | `CNF` is emitted after a successful value change. |
| `IDLE` | The value is unchanged, so no adapter update and no event occur. |

## Application Scenarios

- **Selection between two values:** A single A2X value source can be forwarded to one of two different consumers based on a selector index.
- **Mode-dependent routing:** In an automation application, the same process value can be sent to different control or monitoring modules depending on the operational mode.
- **Adapter-based signal distribution:** The FB can be used as a compact demultiplexing stage in a modular IEC 61499 system where communication uses unidirectional A2X and AUI adapters.
- **Event reduction:** When values are frequently repeated, the change-only behavior avoids unnecessary event processing in the downstream logic.

## Comparison with Similar Blocks

| Block Type | Purpose | Difference |
|------------|---------|------------|
| `A2X_AUI_DEMUX_2` | Demultiplexes one A2X input to two A2X outputs using an AUI index. | Adapter-based, generic, event-on-change only. |
| Typical multiplexer | Combines multiple inputs into one output. | Opposite data flow direction. |
| `AX_AUI_DEMUX_2` | Similar demultiplexing function with an alternative adapter type. | This block is the A2X variant and uses `A2X` for the value channel. |

The main distinguishing feature of `A2X_AUI_DEMUX_2` compared with non-adapter demultiplexers is its clean adapter-based interface and its ability to suppress events when no value change occurs.

## Conclusion

`A2X_AUI_DEMUX_2` is a flexible, generic demultiplexer function block for adapter-based 4diac applications. It routes one A2X input value to one of two outputs using an AUI index, while avoiding unnecessary update events. Its compact adapter interface makes it well suited for modular, event-driven control systems.

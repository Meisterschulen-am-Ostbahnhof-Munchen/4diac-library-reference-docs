# A2X_AUI_DEMUX_4

![A2X_AUI_DEMUX_4](./A2X_AUI_DEMUX_4.svg)

* * * * * * * * * *

## Introduction

A2X_AUI_DEMUX_4 is a generic IEC 61499-2 function block that implements a 1-to-4 demultiplexer for unidirectional adapter connections. It receives a value through an `IN` socket and forwards it to one of four output adapters `OUT1` ... `OUT4`. The active output is selected by the index value received through the `K` socket.

The block is designed as a generic FB. Its runtime behavior is provided by the referenced generic implementation class `GEN_A2X_AUI_DEMUX`. The adapter outputs are only updated when the value actually changes, so unnecessary update events are suppressed.

## Interface Structure

### **Event Inputs**

There are no event inputs.

### **Event Outputs**

| Name | Type | Comment |
|---|---|---|
| `CNF` | Event | Confirmation of Set Index K |

### **Data Inputs**

There are no direct data inputs. All data is exchanged through adapter sockets and plugs.

### **Data Outputs**

There are no direct data outputs. All data is exchanged through adapter sockets and plugs.

### **Adapters**

| Interface Role | Adapter Name | Type | Comment |
|---|---|---|---|
| Socket / Input | `K` | `adapter::types::unidirectional::AUI` | Index |
| Socket / Input | `IN` | `adapter::types::unidirectional::A2X` | Input Value to demultiplex |
| Plug / Output | `OUT1` | `adapter::types::unidirectional::A2X` | Output value 1, selected when `K = 0` |
| Plug / Output | `OUT2` | `adapter::types::unidirectional::A2X` | Output value 2, selected when `K = 1` |
| Plug / Output | `OUT3` | `adapter::types::unidirectional::A2X` | Output value 3, selected when `K = 2` |
| Plug / Output | `OUT4` | `adapter::types::unidirectional::A2X` | Output value 4, selected when `K = 3` |

## Functionality

A2X_AUI_DEMUX_4 acts as a unidirectional adapter-based demultiplexer:

1. The index `K` is received through the `K` socket.
2. The value to be routed is received through the `IN` socket.
3. Depending on `K`, one of the four output adapters `OUT1` ... `OUT4` is selected.
4. The selected output is updated with the incoming value.
5. The `CNF` event confirms that the index setting has been processed.

According to the FB type comment, the adapter output is only updated when the value changes. If the new value is identical to the previous value, no unnecessary output update or event is generated.

Because the FB is a generic FB, the detailed internal processing is delegated to the runtime backend `GEN_A2X_AUI_DEMUX`. The type definition itself mainly describes the adapter interface and the generic class binding.

## Technical Features

- Generic FB implementation: `GEN_A2X_AUI_DEMUX`
- Package: `adapter::selection::unidirectional`
- 1-to-4 demultiplexing using unidirectional adapters
- Four A2X output plugs: `OUT1`, `OUT2`, `OUT3`, `OUT4`
- One AUI index socket: `K`
- One A2X input socket: `IN`
- No direct data inputs or data outputs
- Only one event output: `CNF`
- Output updates and events are suppressed when the value has not actually changed
- Suitable for IEC 61499-2 adapter-based applications

## State Overview

The XML type definition does not contain an explicit ECC. The behavior is therefore controlled by the generic implementation. The following states describe the logical behavior of the FB:

| State | Description |
|---|---|
| Idle | The FB waits for a new index or input value. |
| Select | The index `K` is read and one of the four outputs is selected. |
| Update | The selected output adapter is updated with the value from `IN`. If the value has not changed, this step is skipped. |
| Confirm | The `CNF` event is emitted to confirm that the index selection has been processed. |

## Application Scenarios

A2X_AUI_DEMUX_4 is useful in IEC 61499 applications where a single unidirectional adapter value must be routed to one of four consumers. Typical scenarios include:

- Distributing a measured value to one of several processing blocks.
- Selecting one of four control destinations based on an index.
- Building adapter-based routing structures without using direct data connections.
- Reducing unnecessary communication traffic when the routed value changes infrequently.

## Comparison with Similar Blocks

- A standard demultiplexer typically uses direct data inputs and outputs. A2X_AUI_DEMUX_4 instead uses unidirectional adapters, which allows flexible adapter-based connections.
- It is the A2X adapter variant of the `AX_AUI_DEMUX_4` demultiplexer concept.
- Unlike a simple selector, this FB has one input and four possible output destinations.
- Unlike blocks that always forward data, this FB only updates an output when the value actually changes, reducing unnecessary events.

## Conclusion

A2X_AUI_DEMUX_4 is a compact, generic, adapter-based 1-to-4 demultiplexer. It combines index-based output selection with unidirectional A2X/AUI adapter communication and change-only update behavior. This makes it suitable for efficient and flexible routing in IEC 61499-2 applications.

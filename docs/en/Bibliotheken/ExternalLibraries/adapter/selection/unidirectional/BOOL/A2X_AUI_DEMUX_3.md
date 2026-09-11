# A2X_AUI_DEMUX_3

![A2X_AUI_DEMUX_3](./A2X_AUI_DEMUX_3.svg)

* * * * * * * * * *

## Introduction

A2X_AUI_DEMUX_3 is a generic A2X demultiplexer function block. It receives a value through an A2X input adapter and forwards it to one of three A2X output adapters. The active output is selected by an index value provided through an AUI adapter socket.

The block is designed to reduce unnecessary communication: the adapter output is only updated when the value actually changes, and an event is only generated for a real value change.

## Interface Structure

### **Event Inputs**

The FB does not declare dedicated event inputs on its FB interface. Event interaction is expected through the connected A2X/AUI adapters.

### **Event Outputs**

- `CNF` — Confirmation of Set Index K. This event indicates that the index selection has been accepted and the selected output path is active.

### **Data Inputs**

None. All incoming data is transferred through the adapter sockets.

### **Data Outputs**

None. All outgoing data is transferred through the adapter plugs.

### **Adapters**

Sockets:

- `K` — Adapter type `adapter::types::unidirectional::AUI` — index input.
- `IN` — Adapter type `adapter::types::unidirectional::A2X` — input value to demultiplex.

Plugs:

- `OUT1` — Adapter type `adapter::types::unidirectional::A2X` — output value 1, selected when `K = 0`.
- `OUT2` — Adapter type `adapter::types::unidirectional::A2X` — output value 2, selected when `K = 1`.
- `OUT3` — Adapter type `adapter::types::unidirectional::A2X` — output value 3, selected when `K = 2`.

## Functionality

A2X_AUI_DEMUX_3 acts as a 1-to-3 demultiplexer for A2X adapter values. The index received on the `K` socket selects which of the three output adapters receives the incoming value from the `IN` socket.

The block only forwards an output value if the value has actually changed compared to the last forwarded value. This reduces unnecessary output updates and event traffic. When a new index is applied, the block sends a `CNF` event as confirmation.

## Technical Features

- Generic FB with the generic class name `GEN_A2X_AUI_DEMUX`.
- No explicit data inputs or data outputs; all payload data is carried by A2X adapters.
- Unidirectional adapter interface.
- 1-of-3 demultiplexing.
- Change detection on the output value.
- Event output `CNF` for confirming index changes.
- Suitable for integration into adapter-based 4diac applications.

## State Overview

The FB does not expose an internal ECC state machine in its XML definition. Conceptually, the block behaves as follows:

- **Idle state** — No new index or value has arrived. The selected output keeps its last value.
- **Value update** — A new value arrives on `IN`. If it differs from the previously forwarded value, the selected output adapter is updated and an event is generated.
- **Index change** — A new value arrives on `K`. The next value update is redirected to the newly selected output. The `CNF` event confirms the selection change.

## Application Scenarios

A2X_AUI_DEMUX_3 can be used wherever one input value must be routed to one of several possible consumers. Typical scenarios include:

- Distributing process values to different visualization or control units.
- Switching between multiple output branches based on an operating mode or index.
- Reducing communication load by suppressing unchanged values.
- Building generic adapter-based routing logic in 4diac applications.

## Comparison with Similar Blocks

| Block / Variant | Characteristics |
|---|---|
| `A2X_AUI_DEMUX_3` | Generic A2X-based 1-to-3 demultiplexer with change detection and `CNF` confirmation. |
| `AX_AUI_DEMUX_3` | Closely related variant that does not use the A2X generic adapter backend. |
| Conventional demultiplexer without change detection | Forwards every input value and event unconditionally, causing higher communication load. |

## Conclusion

A2X_AUI_DEMUX_3 is a compact generic demultiplexer block for adapter-oriented 4diac applications. It combines selection of one of three outputs with change-based update behavior, making it suitable for efficient and selective value distribution in automation and communication systems.

# AUI_DEMUX_3

![AUI_DEMUX_3](./AUI_DEMUX_3.svg)

* * * * * * * * * *

## Introduction

The `AUI_DEMUX_3` is an event demultiplexer function block that routes an incoming event to one of three possible event outputs based on a select index. Unlike a conventional demultiplexer (e.g., `E_DEMUX`), the selection information is not provided through a separate data input but via an AUI (Adapter for Unified Interaction) socket. This allows the select value and the triggering event to be delivered together through a single adapter interface, simplifying wiring and enabling a generic, reusable pattern for event routing.

This block is designed for use in IEC 61499‑1 Annex A compliant systems and is intended for scenarios where an event needs to be distributed to different downstream function blocks depending on a runtime‑available code or identifier.

## Interface Structure

### **Event Inputs**

None. The demultiplexing trigger is received via the AUI adapter socket `K`. The adapter encapsulates both the event that initiates the demultiplexing and the index value that determines which output is activated.

### **Event Outputs**

| Output | Type | Comment |
|--------|------|---------|
| `EO1` | Event | Output event when the select value `K` equals `0` |
| `EO2` | Event | Output event when the select value `K` equals `1` |
| `EO3` | Event | Output event when the select value `K` equals `2` |

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

| Adapter | Type | Direction | Comment |
|---------|------|-----------|---------|
| `K` | `adapter::types::unidirectional::AUI` | Socket | Provides the select index and the triggering event. The AUI adapter typically carries an event and an integer data value that is used to select one of the three outputs. |

## Functionality

The `AUI_DEMUX_3` block waits for an event to arrive through the adapter socket `K`. When that event occurs, the block reads the integer value associated with the adapter. The value is used as an index to determine which of the three event outputs shall be issued:

- If the value is `0`, `EO1` is triggered.
- If the value is `1`, `EO2` is triggered.
- If the value is `2`, `EO3` is triggered.

If the value is not one of these three valid indices, no output event is generated (the event is ignored). The block operates in a purely event‑driven manner; it does not store any internal state or maintain the selection between invocations.

Because the select index and the triggering event are delivered together via the adapter, the block can be used as a building block for more complex event‑routing networks without requiring additional data connections or manual synchronization.

## Technical Features

- **Adapter‑Based Input**: The select index and event are packaged into an AUI adapter, promoting a clean interface and reducing the number of separate pins.
- **Generic Implementation**: The block is annotated as a generic function block (`eclipse4diac::core::GenericClassName = 'GEN_E_DEMUX'`). This indicates that the same logic can be instantiated with different adapters or select ranges, although the current façade is fixed to three outputs.
- **No Internal State**: The block is stateless; each invocation is independent and does not depend on previous calls.
- **Standard Compliance**: Conforms to the IEC 61499‑1 Annex A event‑driven model.

## State Overview

`AUI_DEMUX_3` does not maintain any internal state machine. Its behavior is entirely combinational in the event domain: a single event arrives through the adapter, and based on the accompanying data value, exactly one of the three output events is emitted. There are no idle→active transitions or persistent memory elements. This makes the block easy to reason about and suitable for high‑throughput event‑handling applications.

## Application Scenarios

- **Event Routing**: Distributing a single event to one of several processing units based on a runtime‑selected channel ID.
- **Mode Selection**: Triggering different control actions depending on a mode number provided by a supervisor or a higher‑level orchestration block.
- **Protocol Decoding**: In communication stacks, an incoming packet can be demultiplexed to the appropriate handler based on a header field.
- **Test and Simulation**: As a generic demultiplexer, it can be used to simulate different branches of an application by injecting events with configurable indices.

## Comparison with Similar Blocks

| Block | Input Type | Number of Outputs | Select Mechanism |
|-------|------------|-------------------|------------------|
| `E_DEMUX` | Separate event input `EI` and data input `K` | Variable (2, 3, …) | Standard data input |
| `AUI_DEMUX_3` | Adapter socket `K` (combines event + data) | Fixed to 3 | AUI adapter |

The primary difference is the interface. `AUI_DEMUX_3` consolidates the trigger and the selection into a single adapter, which can simplify system wiring and improve modularity. It also imposes a fixed number of outputs (here, three), whereas a generic `E_DEMUX` might be parameterized. However, the underlying demultiplexing behavior—selecting exactly one output based on an index—is identical.

## Conclusion

The `AUI_DEMUX_3` function block provides a concise, adapter‑based solution for event demultiplexing in IEC 61499 applications. By integrating the select index and the triggering event into a single AUI socket, it reduces connection complexity and enhances reusability. Its stateless, event‑driven design fits well into modern automation and distributed control scenarios, and its generic annotation ensures that it can be adapted to various use cases. For systems requiring a clean separation between event triggering and routing logic, `AUI_DEMUX_3` is a recommended building block.

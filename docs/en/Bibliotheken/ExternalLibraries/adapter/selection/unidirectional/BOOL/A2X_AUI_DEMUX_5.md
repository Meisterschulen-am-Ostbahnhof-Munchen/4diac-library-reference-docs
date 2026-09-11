# A2X_AUI_DEMUX_5

![A2X_AUI_DEMUX_5](./A2X_AUI_DEMUX_5.svg)

* * * * * * * * * *

## Introduction

`A2X_AUI_DEMUX_5` is a generic adapter-based demultiplexor. It receives a value through the unidirectional `A2X` socket `IN` and forwards it to one of five unidirectional `A2X` output plugs (`OUT1` ... `OUT5`). The output selection is determined by the index value received through the `AUI` socket `K`.

The block is designed as a generic FB: the concrete type name `A2X_AUI_DEMUX_5` represents a five-output configuration of the generic class `GEN_A2X_AUI_DEMUX`. A key property is that the selected adapter output is updated only when the transferred value actually changes; no unnecessary update event is generated for unchanged values.

## Interface Structure

The FB uses adapter connections instead of a direct set of data inputs and outputs. It has no direct event inputs and no direct data inputs or outputs. All input and output data is carried by the adapter sockets and plugs.

### **Event Inputs**

The FB does not declare any direct event inputs.

### **Event Outputs**

| Event | Description |
| --- | --- |
| `CNF` | Confirmation that the index `K` has been set and the demultiplexing selection has been applied. |

### **Data Inputs**

The FB does not declare any direct data inputs.

### **Data Outputs**

The FB does not declare any direct data outputs.

### **Adapters**

| Direction | Name | Type | Description |
| --- | --- | --- | --- |
| Socket (input) | `K` | `adapter::types::unidirectional::AUI` | Selection index. |
| Socket (input) | `IN` | `adapter::types::unidirectional::A2X` | Input value to demultiplex. |
| Plug (output) | `OUT1` | `adapter::types::unidirectional::A2X` | Output selected when `K = 0`. |
| Plug (output) | `OUT2` | `adapter::types::unidirectional::A2X` | Output selected when `K = 1`. |
| Plug (output) | `OUT3` | `adapter::types::unidirectional::A2X` | Output selected when `K = 2`. |
| Plug (output) | `OUT4` | `adapter::types::unidirectional::A2X` | Output selected when `K = 3`. |
| Plug (output) | `OUT5` | `adapter::types::unidirectional::A2X` | Output selected when `K = 4`. |

## Functionality

The functional behavior can be summarized as:

1. The FB observes the index adapter `K`.
2. Based on the current index, it activates one of the output plugs:
   - `K = 0` selects `OUT1`
   - `K = 1` selects `OUT2`
   - `K = 2` selects `OUT3`
   - `K = 3` selects `OUT4`
   - `K = 4` selects `OUT5`
3. The value received on the `IN` socket is forwarded to the selected output plug.
4. The output adapter is updated only when the value has actually changed. This avoids unnecessary adapter update events.
5. After the selection has been applied, the `CNF` event is emitted.

The valid index range is therefore `0` to `4`. Behavior for indices outside this range depends on the concrete generic runtime implementation.

## Technical Features

- Generic FB class: `GEN_A2X_AUI_DEMUX`.
- Five unidirectional output adapters of type `A2X`.
- Two unidirectional input adapters: `K` (`AUI`) and `IN` (`A2X`).
- Adapter-based interface: no direct data inputs or outputs.
- Value-change-triggered output update.
- Event output `CNF` for confirmation of the index selection.
- Declared as an IEC 61499-2 FB type.
- Compiler package: `adapter::selection::unidirectional`.

## State Overview

The FB type does not contain an explicit ECC state machine. The behavior is therefore handled by the generic runtime implementation. Logically, the following phases can be distinguished:

| Phase | Description |
| --- | --- |
| Wait | The FB waits for a new value on `IN` or a new index on `K`. |
| Select | The current value of `K` determines the active output plug. |
| Forward | The current `IN` value is written to the selected output if it changed. |
| Confirm | `CNF` is emitted after the selection has been applied. |

These phases are not necessarily separate ECC states; they describe the observable behavior of the demultiplexor.

## Application Scenarios

- Routing a single data source to one of five consumers based on an index.
- Dynamic selection of a target station or submodule in an adapter-based automation network.
- Reduction of event load by forwarding only actual value changes.
- Reuse in unidirectional data-flow architectures without direct data wiring.

## Comparison with Similar Blocks

- Compared with a conventional demultiplexer that uses direct data inputs and outputs, `A2X_AUI_DEMUX_5` uses adapter sockets and plugs. This makes it suitable for applications that already model communication endpoints as adapters.
- The `_5` variant is a concrete five-output configuration of the generic class `GEN_A2X_AUI_DEMUX`; other variants can provide a different number of outputs.
- In contrast to a multiplexer, which combines multiple input values into one output, this block distributes one input value to one of several outputs.

## Conclusion

`A2X_AUI_DEMUX_5` is a compact and reusable generic demultiplexor for adapter-based unidirectional communication. Its index-controlled output selection, five outputs, and value-change-triggered update behavior make it suitable for applications where a single value must be routed to one of several destinations without generating unnecessary events.

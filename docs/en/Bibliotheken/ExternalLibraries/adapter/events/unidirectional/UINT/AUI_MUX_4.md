# AUI_MUX_4

![AUI_MUX_4](./AUI_MUX_4.svg)

* * * * * * * * * *
## Introduction

The **AUI_MUX_4** is an event multiplexer function block that selects one of four event inputs and forwards the received event through an **AUI adapter** interface. This is an adapter-based variant of the standard `E_MUX_4` block, replacing the plain event output plus data selector with a unidirectional AUI adapter plug. The block is configured as a generic function block (`GEN_E_MUX`), allowing the number of inputs to be adapted as needed.

## Interface Structure

The AUI_MUX_4 features four event inputs and one AUI adapter plug. There are no conventional event outputs, data inputs, or data outputs; all selection and output functionality is carried through the adapter.

### **Event Inputs**

| Name | Type | Comment |
|------|------|---------|
| EI1  | Event | Event to multiplex, K=0 |
| EI2  | Event | Event to multiplex, K=1 |
| EI3  | Event | Event to multiplex, K=2 |
| EI4  | Event | Event to multiplex, K=3 |

### **Event Outputs**

None. The multiplexed event is delivered via the AUI adapter plug.

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

| Name | Type | Direction | Comment |
|------|------|-----------|---------|
| K    | `adapter::types::unidirectional::AUI` | Plug (output) | Event index and event forwarding interface |

The adapter `K` is of type `AUI` (unidirectional), carrying both the selection index and the event signal to the connected partner FB.

## Functionality

The AUI_MUX_4 operates as an event selector. When an event occurs on one of the four inputs (EI1–EI4), the block recognizes the corresponding index (0–3) and forwards an event through the AUI adapter plug `K`. The adapter carries the index value along with the event trigger, enabling the connected downstream block to identify which input caused the event.

The behavior is analogous to a classic multiplexer: exactly one input is selected and its event is propagated. The selection is implicit—based on which input fired—rather than set via a separate data input. This makes the block suitable for scenarios where event sources are known and need to be routed to a single consumer.

## Technical Features

- **Generic FB instantiation**: Marked with `eclipse4diac::core::GenericClassName` = `'GEN_E_MUX'`, enabling flexible adaptation of the number of event inputs.
- **Adapter-based output**: Uses the `AUI` adapter type (`adapter::types::unidirectional::AUI`) instead of a standard event output plus data output, offering a clean interface for unidirectional event communication.
- **Four input channels**: Supports multiplexing of up to four independent event sources.
- **No data handling**: Pure event-based operation; no data inputs or outputs are involved.
- **Package classification**: Belongs to the `adapter::events::unidirectional` package, reflecting its unidirectional event-oriented architecture.

## State Overview

The block does not maintain internal state machines or persistent states. It operates purely combinatorially in response to incoming events. Each event on an input is immediately forwarded through the adapter without internal buffering or state retention. There are no ECC (Execution Control Chart) states to consider for this FB.

## Application Scenarios

- **Event consolidation**: Merging multiple event sources into a single event stream where the adapter carries the origin index.
- **Inter-module event routing**: Connecting multiple sensors or controllers to a common processing unit via an AUI adapter, preserving the identity of the event source.
- **Generic event demultiplexing**: When used with a matching AUI recipient block, the index can be used to reconstruct the original event distribution on the consumer side.
- **Adapter-based system design**: Ideal for modular IEC 61499 systems that rely on adapter connections for clean, decoupled communication between function blocks.

## Comparison with Similar Blocks

| Feature | AUI_MUX_4 | E_MUX_4 (standard) |
|---------|-----------|--------------------|
| Output type | AUI adapter plug | Event output (EO) + data output (K) |
| Selection mechanism | Implicit via fired input, index via adapter | Explicit via data input K |
| Event count | 4 inputs | 4 inputs |
| Data handling | None | Carries selector value K |
| Use case | Adapter-based architectures | Traditional ECC/event chains |

The key difference is the output interface: AUI_MUX_4 delivers the event and selector via an adapter, promoting loose coupling and reusability, whereas E_MUX_4 uses traditional event and data outputs.

## Conclusion

The AUI_MUX_4 is a specialized event multiplexer designed for adapter-centric IEC 61499 applications. By utilizing a unidirectional AUI adapter for both event forwarding and index signaling, it simplifies wiring in complex systems and enables clean separation of concerns. Its generic nature allows scaling to different input counts, making it a flexible building block for event routing in distributed automation systems.
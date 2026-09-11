# AUI_MUX_7

![AUI_MUX_7](./AUI_MUX_7.svg)

* * * * * * * * * *

## Introduction

The **AUI_MUX_7** is a generic event multiplexer function block that selects one out of seven incoming event inputs and routes it to an event output provided through an **AUI (Adapter Usage Interface)** adapter. This block is a variant of the standard **E_MUX_7**, where the selection index and the output event are bundled into a single unidirectional AUI adapter, making it suitable for modular, adapter-based communication patterns in distributed control applications.

## Interface Structure

### **Event Inputs**

| Name | Type | Description |
|------|------|-------------|
| EI1 | Event | Event to multiplex; selected when K = 0 |
| EI2 | Event | Event to multiplex; selected when K = 1 |
| EI3 | Event | Event to multiplex; selected when K = 2 |
| EI4 | Event | Event to multiplex; selected when K = 3 |
| EI5 | Event | Event to multiplex; selected when K = 4 |
| EI6 | Event | Event to multiplex; selected when K = 5 |
| EI7 | Event | Event to multiplex; selected when K = 6 |

### **Event Outputs**

*None.* The multiplexed event is delivered through the AUI adapter's event channel.

### **Data Inputs**

*None.* The selection index is provided via the AUI adapter.

### **Data Outputs**

*None.*

### **Adapters**

| Name | Type | Direction | Description |
|------|------|-----------|-------------|
| K | `adapter::types::unidirectional::AUI` | Plug (output side) | Carries the event index (selection value) and provides the multiplexed event output to the connected socket. |

## Functionality

The **AUI_MUX_7** operates as a single-selection multiplexer controlled by the index value `K` supplied through the AUI adapter. When an event occurs on one of the seven inputs (EI1–EI7), the block checks the current value of `K`:

- If `K` matches the index associated with the triggered input, the event is forwarded to the output event channel of the AUI adapter.
- If `K` does not match, the event is discarded.

Only one event input is forwarded at a time, determined by the most recent value of `K`. This behavior allows a single consumer connected via the AUI socket to receive events from exactly one source (selected dynamically at runtime) without requiring separate event connections.

## Technical Features

- **Generic FB implementation** – The block is based on the generic class `GEN_E_MUX`, allowing instantiation with a configurable number of inputs (here fixed to 7).
- **Adapter-based output** – The output event and selection index are encapsulated in a unidirectional AUI adapter, promoting loose coupling between producer and consumer.
- **Event-index correlation** – Each event input is mapped to a fixed index (EI1→0, EI2→1, ..., EI7→6), directly corresponding to the `K` value.
- **No data processing** – The block performs purely event-based routing; it does not manipulate or generate data values.
- **SPDX-Licensed** – Distributed under Eclipse Public License 2.0.

## State Overview

The **AUI_MUX_7** is a stateless, combinational event router. It does not maintain internal state variables or sequential logic. The behavior can be summarized as:

| Condition | Action |
|-----------|--------|
| Event on EIn and K == (n-1) | Forward event to AUI output |
| Event on EIn and K != (n-1) | Drop event (no output) |
| No event on any input | No action |

Since there is no state, the block is always ready to react to the next incoming event.

## Application Scenarios

- **Dynamic source selection** – In systems where multiple sensors or controllers generate events but only one should be active at a time, the AUI_MUX_7 can route the active source's events to a downstream processing unit.
- **Adapter-based modular architectures** – When using AUI adapters to standardize connections between components, this block integrates seamlessly with unidirectional event-driven communication patterns.
- **Mode-switchable control panels** – A control application can switch between different operational modes (e.g., manual, automatic, emergency) by changing `K`, thereby selecting which event source (button, PLC signal, safety circuit) drives the output.
- **HMI and event aggregation** – In visualization systems, the block can multiplex multiple user-interface events to a single rendering engine, with the active widget determined by the index.

## Comparison with Similar Blocks

| Feature | AUI_MUX_7 | E_MUX_7 (standard) |
|---------|-----------|---------------------|
| Output event | Via AUI adapter | Plain Event Output (EO) |
| Selection index | Via AUI adapter (K) | Separate data input (K) |
| Interface style | Adapter-based (unidirectional) | Direct event/data pins |
| Coupling | Loose (decoupled via adapter) | Tight (direct wiring) |
| Reusability | Higher – can connect to any matching AUI socket | Lower – requires explicit event and data connections |
| Typical use | Modular, distributed IEC 61499 systems | Simple, monolithic function block networks |

The AUI_MUX_7 is essentially the adapter-wrapped equivalent of the standard E_MUX_7, trading a direct interface for greater modularity and interchangeability.

## Conclusion

The **AUI_MUX_7** provides a clean, adapter-based solution for multiplexing seven event sources into a single output. By integrating the selection index and the output event into one unidirectional AUI adapter, it simplifies wiring, improves component interchangeability, and fits naturally into modern IEC 61499 distributed control architectures that rely on adapter-based communication. Its stateless nature and generic implementation make it a robust and flexible building block for event-driven applications.

# AUI_DEMUX_4

![AUI_DEMUX_4](./AUI_DEMUX_4.svg)

* * * * * * * * * *

## Introduction

The **AUI_DEMUX_4** function block is an event demultiplexer that routes incoming event triggers to one of four distinct output events based on an index value. Unlike the conventional E_DEMUX_4 block, which uses a separate plain event input and a data input for the selector index, this variant receives both the event trigger and the selector value through a single AUI adapter socket. This design reduces wiring complexity and improves interface encapsulation in distributed IEC 61499 applications. The block is implemented as a generic FB (GEN_E_DEMUX) and is part of the `adapter::events::unidirectional` package.

## Interface Structure

The block provides a minimal interface that contains only event outputs and one adapter socket. There are no direct event inputs, data inputs, or data outputs — all incoming information is carried by the AUI adapter.

### **Event Inputs**

None. The triggering event is delivered via the **K** adapter socket instead of a dedicated event input.

### **Event Outputs**

| Name | Description |
|------|-------------|
| **EO1** | Demultiplexed output, triggered when the index value K = 0 |
| **EO2** | Demultiplexed output, triggered when the index value K = 1 |
| **EO3** | Demultiplexed output, triggered when the index value K = 2 |
| **EO4** | Demultiplexed output, triggered when the index value K = 3 |

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

| Name | Type | Description |
|------|------|-------------|
| **K** | `adapter::types::unidirectional::AUI` (Socket) | Event index adapter. Carries an event trigger together with the selector value (0–3) that determines which output is activated. |

## Functionality

The **AUI_DEMUX_4** block implements a 1-to-4 event demultiplexing function. The AUI adapter **K** encapsulates both an event signal and an index value. When an event arrives through the adapter, the block evaluates the accompanying index value and forwards the event to the corresponding output:

- Index **0** → event emitted on **EO1**
- Index **1** → event emitted on **EO2**
- Index **2** → event emitted on **EO3**
- Index **3** → event emitted on **EO4**

Only one output event is triggered per incoming event. The block acts as a pure event router — no data is transformed or stored, and no additional processing is performed.

## Technical Features

- **Generic FB implementation**: Declared with `GenericClassName = 'GEN_E_DEMUX'`, making it a specialization of the generic E_DEMUX pattern within the 4diac framework.
- **Adapter-based event transport**: The combined event-and-index interface simplifies connections and improves reusability across different application contexts.
- **Unidirectional adapter**: The AUI adapter operates in one direction (socket side), suitable for producer-consumer event flows.
- **Deterministic routing**: The index-to-output mapping is fixed and immediate; no memory or state retention is involved in the routing decision.
- **Compliance**: Conforms to IEC 61499-1 Annex A identification and is distributed under the Eclipse Public License 2.0.

## State Overview

The **AUI_DEMUX_4** block is essentially stateless in terms of routing logic. Its behavior can be described by a simple state model:

- **Idle State**: The block waits for an event to arrive on the **K** adapter.
- **Routing State**: Upon receiving an event, the index value is captured and the event is dispatched to the matching output (EO1–EO4). After emission, the block returns to the Idle State.

Since the block processes each event independently, there is no internal state that persists between activations.

## Application Scenarios

- **Event distribution in modular control systems**: Use the block to route events from a single source to different consumers (e.g., state machines, processing stages) based on a dynamically changing selector.
- **Command dispatch**: In a machine control application, the AUI adapter can carry a command code (0–3) along with an execution trigger, directing the command to one of four sub-controllers.
- **Communication protocol handling**: When a received telegram contains a channel or service identifier, this block can forward the associated event to the appropriate protocol handler.
- **Reduced wiring in distributed setups**: By bundling event and index into a single adapter, connections between function blocks become cleaner and more maintainable, especially in large IEC 61499 system configurations.

## Comparison with Similar Blocks

| Feature | **AUI_DEMUX_4** | **E_DEMUX_4 (Standard)** |
|---------|-----------------|--------------------------|
| Event input | Via AUI adapter | Separate EI event input |
| Selector input | Part of adapter (index) | Separate K data input |
| Data inputs | None | 1 (K) |
| Data outputs | None | None |
| Interface complexity | Reduced (fewer connections) | Higher (multiple separate pins) |
| Genericity | Generic FB (GEN_E_DEMUX) | Standard non-generic FB |
| Use case | Clean adapter-oriented designs | Classic flat wiring designs |

The key advantage of the adapter variant is interface consolidation: event and index travel together, which improves modularity and reduces the risk of connection mismatches in complex applications.

## Conclusion

The **AUI_DEMUX_4** function block provides a compact and elegant solution for event demultiplexing in IEC 61499-based systems. By integrating the event trigger and the selector index into a single AUI adapter, it simplifies the interface while preserving the deterministic routing behavior of a classic E_DEMUX. It is well suited for modular, adapter-centric control architectures where reducing wiring complexity and improving encapsulation are important design goals.

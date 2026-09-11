# AUI_MUX_2

![AUI_MUX_2](./AUI_MUX_2.svg)

* * * * * * * * * *

## Introduction

AUI_MUX_2 is an event multiplexer function block that forwards one of two incoming events (EI1 or EI2) based on an index value supplied through an AUI (Adapter User Interface) adapter. It is a specialized variant of the standard E_MUX_2 block, replacing the conventional combination of a data input `K` and a plain event output `EO` with a unidirectional AUI adapter connection. This design integrates the event-index selection and the multiplexed event delivery into a single adapter-based interface, making it suitable for architectures where adapter-based communication is preferred over direct event/data ports.

## Interface Structure

### **Event Inputs**

| Name | Type | Description |
|------|------|-------------|
| `EI1` | Event | Event to be multiplexed when the index K selects channel 0. |
| `EI2` | Event | Event to be multiplexed when the index K selects channel 1. |

### **Event Outputs**

The block has no explicit event output ports. The multiplexed event is delivered through the AUI adapter connection (see Adapters below).

### **Data Inputs**

No data inputs are provided. All selection information is carried via the adapter.

### **Data Outputs**

No data outputs are provided. The adapter serves as the data and event output mechanism.

### **Adapters**

| Name | Type | Direction | Description |
|------|------|-----------|-------------|
| `K` | `adapter::types::unidirectional::AUI` | Plug (required) | Provides the event selection index and carries the multiplexed event output to a connected socket. |

## Functionality

The AUI_MUX_2 operates as a two-channel event selector. When an event arrives on `EI1` or `EI2`, the block evaluates the current index value transmitted through the `K` adapter. If the index selects channel 0, the incoming event on `EI1` is forwarded via the AUI adapter to the connected partner FB. If the index selects channel 1, the event on `EI2` is forwarded instead. Events arriving on a non-selected channel are ignored.

The adapter-based design encapsulates both the index value and the event output in one interface. This means the consuming logic does not need to observe a separate `K` data input or a dedicated event output; instead, it receives the selection context and the event through the same AUI connection. This reduces port clutter and enforces a cleaner separation between the multiplexing logic and the consuming application.

The block is declared as a generic FB (`GEN_E_MUX`) within the 4diac framework, meaning its behavior can be instantiated with a configurable number of inputs, although the fixed `AUI_MUX_2` variant presented here exposes exactly two event input channels.

## Technical Features

- **Adapter-based I/O:** All selection and output communication occur through a unidirectional AUI adapter, eliminating dedicated data input and event output ports.
- **Generic background:** Built on the generic `GEN_E_MUX` core, allowing conceptual extension to more than two channels in adapted versions.
- **Combinational event logic:** No internal storage or state machine is required; the routing decision is made purely based on the current adapter index value at the time an input event occurs.
- **Deterministic selection:** Each input event channel is strictly associated with one index value (EI1 ↔ index 0, EI2 ↔ index 1).
- **Standard compliance:** Developed according to IEC 61499-1 Annex A, ensuring interoperability within 4diac-based automation systems.

## State Overview

This function block does not maintain persistent internal states. Its behavior is purely event-driven and combinational:

- When `EI1` fires and the adapter-index is 0 → the selected event is emitted through the AUI adapter.
- When `EI1` fires and the adapter-index is not 0 → the event is discarded.
- When `EI2` fires and the adapter-index is 1 → the selected event is emitted through the AUI adapter.
- When `EI2` fires and the adapter-index is not 1 → the event is discarded.

The block's reaction is instantaneous and does not depend on prior events, making it a deterministic routing element within a larger IEC 61499 application.

## Application Scenarios

AUI_MUX_2 is particularly useful in system designs that already rely on AUI adapter connections for unidirectional communication between FBs. Typical use cases include:

- **Event routing in adapter-based architectures:** When a framework or application layer mandates that all event exchange between components occurs via adapters, AUI_MUX_2 allows event selection without breaking that convention.
- **Reducing port complexity:** In large FB networks, replacing multiple separate `K` data inputs and `EO` outputs with a single adapter simplifies wiring and improves readability.
- **Modular consumer integration:** The AUI socket on the consumer side can be designed to automatically interpret the multiplexed events with the accompanying index, enabling polymorphic event handling.
- **Migration of E_MUX_2 logic:** Applications previously built with E_MUX_2 but moving toward adapter-based communication can be migrated by swapping the plain output/data ports with the AUI adapter.

## Comparison with Similar Blocks

| Feature | E_MUX_2 | AUI_MUX_2 |
|---------|---------|-----------|
| Event inputs | EI1, EI2 | EI1, EI2 |
| Selection mechanism | Data input `K` (INT) | AUI adapter `K` |
| Event output | Plain event output `EO` | Via AUI adapter |
| Port count | 3 (2 events + 1 data) + 1 event output | 2 events + 1 adapter |
| Adapter support | None | Unidirectional AUI |
| Generic base | GEN_E_MUX | GEN_E_MUX |

The key distinction is that AUI_MUX_2 consolidates the index selection and event output into one adapter interface. This makes it more suitable for adapter-oriented designs but requires that the connected consumer FB exposes a matching AUI socket. In contrast, E_MUX_2 uses the more traditional separate data input and event output, which is simpler to use in classic IEC 61499 applications.

## Conclusion

AUI_MUX_2 provides a clean, adapter-based alternative to the classical E_MUX_2 event multiplexer. By routing the selection index and the multiplexed event through a single unidirectional AUI adapter, it aligns with modern IEC 61499 communication patterns that favor adapter encapsulation. It is ideal for systems where adapter-based event propagation is a design requirement, and it retains the deterministic, combinational behavior expected from a multiplexing element. While slightly more constrained than its plain counterpart due to the dependency on a matching AUI socket, it offers significant advantages in modularity and interface clarity for appropriate application contexts.

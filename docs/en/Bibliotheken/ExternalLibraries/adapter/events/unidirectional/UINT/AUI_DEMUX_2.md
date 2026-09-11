# AUI_DEMUX_2

![AUI_DEMUX_2](./AUI_DEMUX_2.svg)

* * * * * * * * * *
## Introduction
AUI_DEMUX_2 is a generic event demultiplexer function block that routes an incoming event to one of two output event ports based on an index value received through an AUI (Adapter Unified Interface) adapter. It is the adapter-based variant of the standard E_DEMUX_2 block, replacing the conventional event input and data input pair with a single unidirectional AUI adapter. This design allows the event trigger and the selection index to be delivered together as a cohesive bundle, simplifying composition and promoting type-safe connections in IEC 61499 systems.

## Interface Structure
The block features a minimal interface consisting solely of two event outputs and one adapter socket. No separate event inputs or data inputs/outputs are present, as all incoming information arrives via the AUI adapter.

### **Event Inputs**
None. The triggering event is received through the AUI adapter socket "K".

### **Event Outputs**
| Name | Type | Comment |
|------|------|---------|
| EO1  | Event | Output, demultiplexed from EI when K=0 |
| EO2  | Event | Output, demultiplexed from EI when K=1 |

### **Data Inputs**
None. The selection value is conveyed through the adapter.

### **Data Outputs**
None.

### **Adapters**
| Name | Type | Direction | Comment |
|------|------|-----------|---------|
| K    | adapter::types::unidirectional::AUI | Socket | Event index used for demultiplexing |

## Functionality
The block operates as a two-way event demultiplexer. When an event arrives through the AUI adapter K, the associated index value carried by the adapter is evaluated:

- If the index value equals 0, the event is propagated to output **EO1**.
- If the index value equals 1, the event is propagated to output **EO2**.

The AUI adapter bundles an event trigger with a data value, enabling the selection criterion to be delivered atomically with the event itself. This ensures that the demultiplexing decision is always based on the most current index value synchronized with the triggering event.

## Technical Features
- **Generic FB**: Declared with the generic class name `GEN_E_DEMUX`, allowing the compiler/runtime to instantiate it with the appropriate underlying implementation.
- **Adapter-based input**: The single AUI socket replaces the traditional separate event input (EI) and data input (K) of the standard E_DEMUX_2 block.
- **Type-safe connections**: The adapter type enforces correct wiring at design time, preventing mismatched event/data pairs.
- **Two-way demultiplexing**: Supports exactly two distinct output paths based on a binary index.
- **Unidirectional adapter**: The AUI (adapter types::unidirectional::AUI) socket is of the unidirectional kind, meaning data flows in one direction only (into the FB).

## State Overview
AUI_DEMUX_2 does not maintain internal state. It is a purely combinatorial event router:

- On adapter event arrival with index = 0 → EO1 fires.
- On adapter event arrival with index = 1 → EO2 fires.

No internal variables, timers, or memory are used; the behavior is fully deterministic and stateless.

## Application Scenarios
- **Event routing in automation chains**: Distributing a single trigger to different processing branches depending on a mode or selector value.
- **Mode switching**: Selecting between two alternative algorithms or control strategies based on a command index.
- **Multiplexed communication**: Demultiplexing incoming events from a unidirectional AUI-based communication channel into separate functional paths.
- **Structured composition**: Used in systems where adapters are preferred over raw event/data pairs for cleaner, more maintainable type-safe connections.

## Comparison with Similar Blocks
| Feature | AUI_DEMUX_2 | E_DEMUX_2 (standard) |
|---------|-------------|----------------------|
| Input style | Single AUI adapter socket | Separate EI (event) + K (data) |
| Selector | Via adapter data | Via data input K |
| Outputs | EO1, EO2 | EO1, EO2 |
| Generic nature | Yes (GEN_E_DEMUX) | No (fixed type) |
| Wiring complexity | Reduced (one connection) | Higher (two connections) |
| Type safety | Enhanced via adapter contract | Manual, type-checked per pin |

The primary advantage of AUI_DEMUX_2 over E_DEMUX_2 is the consolidation of event and selector into a single adapter interface, which reduces wiring overhead and enforces a consistent data/event pairing at the type level.

## Conclusion
AUI_DEMUX_2 provides a clean, adapter-based solution for two-way event demultiplexing in IEC 61499 systems. By leveraging the AUI adapter pattern, it simplifies interface design, improves type safety, and reduces connection count compared to its classical counterpart. It is particularly well suited for modern, modular automation architectures where adapters are the preferred means of interconnecting function blocks.
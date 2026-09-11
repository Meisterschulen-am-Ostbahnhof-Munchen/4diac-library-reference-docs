# AUI_DEMUX_5

![AUI_DEMUX_5](./AUI_DEMUX_5.svg)

* * * * * * * * * *
## Introduction

The **AUI_DEMUX_5** function block is an event demultiplexer with a five-channel output structure. Instead of using a plain event input (EI) combined with a data selector (K), this block accepts its input through an **AUI adapter** (unidirectional application user interface). The block routes an incoming event to exactly one of five event outputs, based on the indexing value carried by the adapter. It is the adapter-based variant of the standard `E_DEMUX_5` block and is implemented as a generic FB (generic class name `GEN_E_DEMUX`).

## Interface Structure

### **Event Inputs**

There are no direct event inputs on this block. All event triggering and selection information is delivered through the **AUI adapter socket** named `K`. The adapter carries both the incoming event and the associated index value.

### **Event Outputs**

| Name | Type | Comment |
|------|------|---------|
| EO1  | Event | Output, demultiplexed from EI when K=0 |
| EO2  | Event | Output, demultiplexed from EI when K=1 |
| EO3  | Event | Output, demultiplexed from EI when K=2 |
| EO4  | Event | Output, demultiplexed from EI when K=3 |
| EO5  | Event | Output, demultiplexed from EI when K=4 |

### **Data Inputs**

There are no direct data inputs. The selection value is embedded within the AUI adapter data channel.

### **Data Outputs**

There are no data outputs.

### **Adapters**

| Name | Type | Direction | Comment |
|------|------|-----------|---------|
| K    | `adapter::types::unidirectional::AUI` | Socket | Event index (combines event input and selector data) |

## Functionality

The `AUI_DEMUX_5` block receives an event together with a selector index through the AUI adapter `K`. When an event arrives on the adapter's event channel, the block evaluates the accompanying index value and forwards the event to the corresponding output:

- If the index value is **0**, the event is emitted on `EO1`.
- If the index value is **1**, the event is emitted on `EO2`.
- If the index value is **2**, the event is emitted on `EO3`.
- If the index value is **3**, the event is emitted on `EO4`.
- If the index value is **4**, the event is emitted on `EO5`.

Exactly one output is triggered per incoming event; all other outputs remain inactive. This demultiplexing behavior enables the block to selectively activate different downstream logic branches based solely on the index provided through the adapter.

## Technical Features

- **Adapter-based input:** Eliminates the need for a separate event input and data input pair, packaging both into a single `AUI` socket for cleaner interface design.
- **Unidirectional interface:** The adapter direction is unidirectional, meaning data and events flow only into the block.
- **Generic implementation:** Marked with the generic class name `GEN_E_DEMUX`, allowing the block to be instantiated with additional type information if required by the embedding application.
- **Five independent output channels:** Events are routed with a one-hot output pattern, which is straightforward to analyze and integrate into structured control logic.
- **No internal state:** The block operates purely combinatorially on the event transition; it retains no runtime data between events beyond the immediate routing decision.

## State Overview

The `AUI_DEMUX_5` block does not maintain any persistent internal states. Its behavior is instantaneous and deterministic:

1. **Idle** – waiting for an event on the adapter.
2. **Routing** – upon event arrival, the index is evaluated and the matching output is triggered in the same execution cycle.

Because the block is stateless, it is inherently safe to use in multithreaded or event-driven IEC 61499 runtime environments without requiring synchronization measures.

## Application Scenarios

- **Event stream splitting:** Distributing events from a single producer to one of several consumers based on a routing key.
- **Mode-based control:** Selecting between different operational modes (e.g., startup, normal, maintenance, shutdown) by routing the control event to the appropriate handler.
- **Protocol adaptation:** In communication stacks, demultiplexing incoming frames or messages to dedicated processing functions depending on the message type identifier carried in the adapter.
- **Test harnesses:** Building structured test sequences where a single test trigger can be directed to various test case blocks based on an index.

## Comparison with Similar Blocks

| Feature | AUI_DEMUX_5 | E_DEMUX_5 |
|---------|-------------|-----------|
| Input interface | Single AUI adapter socket (event + index combined) | Separate event input (EI) and data input (K) |
| Interface complexity | Reduced – adapter encapsulates both signals | Explicit – requires two distinct connections |
| Suitability for structured models | Better for adapter-based design patterns | Simpler for flat FB networks |
| Index range | 0–4 (five outputs) | 0–4 (five outputs) |
| Event routing logic | Identical | Identical |

The core demultiplexing logic is identical to that of `E_DEMUX_5`. The difference lies exclusively in the input packaging – the adapter approach promotes reusability and reduces wiring clutter when an AUI-based connection pattern is already established in the system architecture.

## Conclusion

`AUI_DEMUX_5` provides a clean, adapter-driven event demultiplexing solution with five outputs. Its integration of the event and index signal into a single `AUI` socket simplifies component interfaces and aligns well with modern, adapter-based IEC 61499 design practices. Being stateless and generic, it is a versatile building block for event routing in distributed automation and control applications.
# AUI_MUX_8

![AUI_MUX_8](./AUI_MUX_8.svg)

* * * * * * * * * *

## Introduction

AUI_MUX_8 is an event multiplexer function block that selects one of eight event inputs and forwards the event through an AUI adapter connection. Instead of exposing a plain event output and a separate selection/data pin, this block uses an AUI adapter named `K` to carry the multiplexed event together with the associated event index.

The block is designed as a generic event multiplexer and is particularly useful when the surrounding application already uses AUI-based unidirectional adapter connections.

## Interface Structure

### **Event Inputs**

| Name | Type | Comment |
|------|------|---------|
| EI1 | Event | Event to multiplex, K=0 |
| EI2 | Event | Event to multiplex, K=1 |
| EI3 | Event | Event to multiplex, K=2 |
| EI4 | Event | Event to multiplex, K=3 |
| EI5 | Event | Event to multiplex, K=4 |
| EI6 | Event | Event to multiplex, K=5 |
| EI7 | Event | Event to multiplex, K=6 |
| EI8 | Event | Event to multiplex, K=7 |

### **Event Outputs**

No event outputs are declared on the function block itself. The multiplexed event is provided through the AUI adapter `K`.

### **Data Inputs**

No data inputs are declared.

### **Data Outputs**

No data outputs are declared.

### **Adapters**

| Name | Type | Direction | Comment |
|------|------|-----------|---------|
| K | `adapter::types::unidirectional::AUI` | Plug | Event index |

The adapter `K` replaces the conventional `EO + K` interface of a standard event multiplexer. It combines the event routing result and the event index information in a single AUI connection.

## Functionality

AUI_MUX_8 multiplexes eight incoming events into one AUI adapter output. When an event occurs on one of the inputs `EI1` to `EI8`, the block forwards the event through the adapter `K` and provides the corresponding index value:

| Event Input | Adapter Index |
|-------------|---------------|
| EI1 | 0 |
| EI2 | 1 |
| EI3 | 2 |
| EI4 | 3 |
| EI5 | 4 |
| EI6 | 5 |
| EI7 | 6 |
| EI8 | 7 |

The receiving function block or subapplication connected to the AUI socket can evaluate the index to determine which event source triggered the multiplexed event.

## Technical Features

- Eight event input channels with fixed index mapping from 0 to 7.
- AUI adapter plug `K` instead of separate event output and data pins.
- Unidirectional adapter type, suitable for event-oriented connections.
- Generic FB handling via the Eclipse 4diac generic class name `GEN_E_MUX`.
- No explicit data inputs or data outputs on the FB itself.
- Stateless operation: every incoming event is handled individually and forwarded through the adapter.

## State Overview

This function block does not define an explicit state machine or internal state chart. It behaves as a stateless event multiplexer. Each event input is processed independently, and the resulting event is immediately forwarded through the AUI adapter `K`.

## Application Scenarios

AUI_MUX_8 is suitable for applications where:

- Multiple event sources must be combined into one event path.
- The downstream components are already connected using AUI adapters.
- A compact adapter-based interface is preferred over separate event output and index data pins.
- Event-driven communication between different parts of an automation solution must be unified.

Typical examples include sensor event aggregation, selection of several command sources, and routing of event signals inside modular 4diac applications.

## Comparison with Similar Blocks

Compared to a standard `E_MUX`, `AUI_MUX_8` uses an AUI adapter output instead of a plain event output `EO` and a separate selection/index input `K`. This reduces the number of separate interface elements on the FB and integrates the event index into the adapter connection.

Compared to demultiplexer blocks such as `E_DEMUX`, the direction of operation is reversed: `AUI_MUX_8` combines several event inputs into one adapter output, while a demultiplexer distributes one event input to several outputs.

## Conclusion

AUI_MUX_8 is a compact, adapter-based event multiplexer for eight input events. By integrating the event index into an AUI unidirectional adapter, it provides a clean and reusable interface for event-driven 4diac applications. Its stateless behavior, clear channel mapping, and generic FB support make it a practical building block for structured event handling.
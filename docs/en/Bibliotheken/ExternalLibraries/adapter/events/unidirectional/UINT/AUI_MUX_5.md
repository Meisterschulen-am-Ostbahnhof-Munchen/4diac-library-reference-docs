# AUI_MUX_5

![AUI_MUX_5](./AUI_MUX_5.svg)

* * * * * * * * * *

## Introduction

`AUI_MUX_5` is a 5-input event multiplexer function block. It receives events on one of five event inputs and forwards the selected event through an AUI adapter output. It is an adapter-based variant of the standard `E_MUX_5` and replaces the usual separate event output `EO` and data input `K` with a single unidirectional AUI adapter interface. This keeps the event output and index information bundled together and simplifies wiring between connected function blocks.

## Interface Structure

### **Event Inputs**

| Event   | Description                  |
|---------|------------------------------|
| `EI1`   | Event to multiplex, K=0      |
| `EI2`   | Event to multiplex, K=1      |
| `EI3`   | Event to multiplex, K=2      |
| `EI4`   | Event to multiplex, K=3      |
| `EI5`   | Event to multiplex, K=4      |

### **Event Outputs**

The block has no direct event output. The multiplexed event is provided through the AUI adapter plug `K`.

### **Data Inputs**

The block has no explicit data inputs. The selection index is carried by the AUI adapter port `K`.

### **Data Outputs**

The block has no explicit data outputs. Index information, if required by the connected block, is available through the AUI adapter.

### **Adapters**

| Adapter | Type                                   | Description            |
|---------|----------------------------------------|------------------------|
| `K`     | `adapter::types::unidirectional::AUI`  | Event index            |

`K` is a plug of the unidirectional AUI adapter type. It acts as the output interface that replaces the separate event output and `K` data input of a conventional event multiplexer.

## Functionality

`AUI_MUX_5` behaves as a 5-to-1 event multiplexer. An event arriving on one of the event inputs is forwarded only when the current index value matches that input:

- `EI1` is selected when K=0
- `EI2` is selected when K=1
- `EI3` is selected when K=2
- `EI4` is selected when K=3
- `EI5` is selected when K=4

If an event arrives on an input whose index does not match the current selection, the event is not forwarded. The AUI adapter `K` supplies the event index and also carries the output event to a connected block with a matching AUI socket.

## Technical Features

- Fixed 5-input specialization of the generic event multiplexer class `GEN_E_MUX`.
- Uses the unidirectional AUI adapter type from `adapter::types::unidirectional`.
- Combines event output and index selection into a single adapter connection.
- Designed for event-based multiplexing according to IEC 61499 event semantics.
- No explicit data inputs or data outputs.
- No internal stored data or persistent memory.
- Available under the Eclipse Public License 2.0.

## State Overview

`AUI_MUX_5` does not define an explicit ECC state machine. It is an event-triggered block with no persistent internal state. Each event invocation is processed by checking the current index on the AUI adapter and, if the index matches, forwarding the event immediately. After processing, the block returns to an idle state.

## Application Scenarios

- Selecting one of five event sources based on a runtime index.
- Connecting to application components that expect a unidirectional AUI event interface.
- Reducing connection complexity in event routing by bundling the event output and index in a single adapter.
- Implementing mode or channel selection in event-driven control applications.

## Comparison with Similar Blocks

| Block               | Difference                                                                 |
|---------------------|----------------------------------------------------------------------------|
| `E_MUX_5`           | Standard version with a separate event output `EO` and data input `K`. `AUI_MUX_5` uses an AUI adapter to carry both. |
| `GEN_E_MUX`         | Generic version supports a configurable number of event inputs. `AUI_MUX_5` is a fixed 5-input specialization. |

## Conclusion

`AUI_MUX_5` provides the familiar behavior of an event multiplexer through a compact adapter-based interface. It is well suited for applications where a single unidirectional AUI connection is preferred over separate event and data ports, while preserving the clear five-input multiplexing logic of the standard `E_MUX` family.
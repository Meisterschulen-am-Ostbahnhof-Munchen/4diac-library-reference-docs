# AUI_MUX_5

![AUI_MUX_5](./AUI_MUX_5.svg)

* * * * * * * * * *

## Introduction

AUI_MUX_5 is an event multiplexer with five event inputs. It is the adapter-based variant of a classic `E_MUX_5` block. Instead of providing a plain event output `EO` and a data output `K`, this block uses a unidirectional AUI adapter output called `K` that carries both the generated event and the selected event index.

The block belongs to the `adapter::events::unidirectional` package and is implemented using the generic event multiplexer class `GEN_E_MUX`.

## Interface Structure

### **Event Inputs**

| Name | Type | Comment |
|------|------|---------|
| `EI1` | Event | Event to multiplex, K=0 |
| `EI2` | Event | Event to multiplex, K=1 |
| `EI3` | Event | Event to multiplex, K=2 |
| `EI4` | Event | Event to multiplex, K=3 |
| `EI5` | Event | Event to multiplex, K=4 |

### **Event Outputs**

None. The event output is embedded in the AUI adapter `K`.

### **Data Inputs**

None.

### **Data Outputs**

None. The data output is embedded in the AUI adapter `K`.

### **Adapters**

| Name | Direction | Type | Comment |
|------|-----------|------|---------|
| `K` | Plug | `adapter::types::unidirectional::AUI` | Event index |

The adapter plug `K` is the output side of the block. It provides a unidirectional connection to a consuming FB that has a matching AUI socket.

## Functionality

AUI_MUX_5 works as an event selector. When one of the five event inputs `EI1` to `EI5` is triggered, the block:

1. Recognizes which event input received the event.
2. Maps that input to the corresponding zero-based index:
   - `EI1` → `K = 0`
   - `EI2` → `K = 1`
   - `EI3` → `K = 2`
   - `EI4` → `K = 3`
   - `EI5` → `K = 4`
3. Forwards the event through the AUI adapter `K`.
4. Provides the index value through the same adapter connection.

The consumer connected to the AUI socket receives both the event and the associated channel index in one structured interface.

## Technical Features

- Five event inputs: `EI1` to `EI5`.
- One unidirectional AUI adapter plug named `K`.
- Generic implementation based on `GEN_E_MUX`.
- Uses the adapter type `adapter::types::unidirectional::AUI`.
- Replaces the separate `EO` and `K` outputs of a conventional event multiplexer.
- The adapter output is unidirectional, meaning events and index data flow from this block to the connected consumer.
- No separate event output, data input, or data output is exposed on the outer interface.

## State Overview

AUI_MUX_5 does not expose an application-visible state machine. It behaves as an event-driven selector. After an event is processed, the selected index is available on the AUI adapter and the event is propagated to the connected socket.

The block is stateless between event occurrences from the user's perspective. No internal event history is required for normal operation.

## Application Scenarios

AUI_MUX_5 is suitable for applications where several event sources need to be combined into one adapter-based output channel.

Typical use cases include:

- Merging event signals from multiple sensors into one common event handling path.
- Selecting one of several alarm or status sources and forwarding it to a central monitoring block.
- Connecting an event multiplexer to an AUI-compatible consumer without manually wiring an event output and an index data output.
- Replacing a conventional `E_MUX_5` in systems that already use unidirectional AUI adapters for communication between FBs.

## Comparison with Similar Blocks

Compared with `E_MUX_5`, AUI_MUX_5 provides the same event multiplexing behavior but uses an AUI adapter output instead of a separate event output `EO` and data output `K`. This groups the related output information together and reduces connection clutter.

Compared with other `AUI_MUX_n` variants, AUI_MUX_5 is specialized for exactly five event inputs. The index mapping starts at `0` and ends at `4`.

Compared with the generic `GEN_E_MUX` block, AUI_MUX_5 is a concrete typed specialization with a fixed number of five event inputs and a predefined AUI adapter interface.

## Conclusion

AUI_MUX_5 is a compact, adapter-based event multiplexer for five event inputs. It combines event selection and index information in one unidirectional AUI adapter connection. This makes it especially useful in component-oriented IEC 61499 applications where standardized AUI adapters are used for event and data exchange between function blocks.
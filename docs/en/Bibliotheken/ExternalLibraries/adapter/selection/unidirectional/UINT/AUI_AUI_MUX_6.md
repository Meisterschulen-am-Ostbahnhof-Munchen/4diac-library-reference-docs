# AUI_AUI_MUX_6

![AUI_AUI_MUX_6](./AUI_AUI_MUX_6.svg)

* * * * * * * * * *

## Introduction

AUI_AUI_MUX_6 is an adapter-based multiplexer function block. It selects one of six unidirectional AUI adapter inputs, `IN1` through `IN6`, based on the index value received through the `K` adapter. The selected value is forwarded to the `OUT` adapter plug. The output is updated only when the actual value changes, and the `CNF` event is issued only after such a real change has occurred.

## Interface Structure

### **Event Inputs**

None. The FB does not expose dedicated event inputs. Its operation is triggered through the connected adapter interfaces, in particular through changes on the `K` index adapter.

### **Event Outputs**

| Name | Type | Description |
|------|------|-------------|
| `CNF` | Event | Confirmation that the index value `K` has been evaluated and the output selection has been processed. In this FB, `CNF` is emitted only when the output value has actually changed. |

### **Data Inputs**

None. Data is transferred through the AUI adapter connections instead of dedicated data inputs.

### **Data Outputs**

None. The selected value is made available through the `OUT` adapter plug.

### **Adapters**

| Direction | Name | Adapter Type | Description |
|-----------|------|--------------|-------------|
| Socket | `K` | `adapter::types::unidirectional::AUI` | Index adapter. Determines which input is selected. |
| Socket | `IN1` | `adapter::types::unidirectional::AUI` | Input value 1, selected when `K = 0`. |
| Socket | `IN2` | `adapter::types::unidirectional::AUI` | Input value 2, selected when `K = 1`. |
| Socket | `IN3` | `adapter::types::unidirectional::AUI` | Input value 3, selected when `K = 2`. |
| Socket | `IN4` | `adapter::types::unidirectional::AUI` | Input value 4, selected when `K = 3`. |
| Socket | `IN5` | `adapter::types::unidirectional::AUI` | Input value 5, selected when `K = 4`. |
| Socket | `IN6` | `adapter::types::unidirectional::AUI` | Input value 6, selected when `K = 5`. |
| Plug | `OUT` | `adapter::types::unidirectional::AUI` | Selected AUI output. Represents `IN1` for `K = 0` up to `IN6` for `K = 5`. |

## Functionality

AUI_AUI_MUX_6 behaves as a multiplexer for AUI adapter connections. It reads the value provided through the `K` socket and interprets it as an integer index between `0` and `5`. Based on this index, one of the six input adapters is routed to the `OUT` adapter.

The FB is designed to avoid unnecessary communication. If a new index value is received but the selected output value has not actually changed, the output is not rewritten and no event is generated. Only when a real value change occurs is the `OUT` adapter updated and the `CNF` event emitted.

Because `K` is itself an AUI adapter, the index can be supplied by another adapter-compatible block, making the FB suitable for use in adapter-oriented IEC 61499 architectures.

## Technical Features

- Six unidirectional AUI input sockets and one unidirectional AUI output plug.
- No direct event inputs or data pins; all communication is performed via adapter interfaces.
- Index range `0` to `5` selects `IN1` to `IN6`.
- Output update and `CNF` event generation are suppressed when the value has not changed.
- The FB is defined as a generic block with the generic class name `GEN_AUI_AUI_MUX`.
- The block is distributed under the Eclipse Public License 2.0.

## State Overview

The FB does not contain an explicit ECC state machine in its type definition. Its behavior can therefore be described conceptually:

| State | Description |
|-------|-------------|
| Idle | Waiting for a new index value or value change on the selected input. |
| Evaluating | The `K` adapter value is read and interpreted as the selection index. |
| Updating | The selected input value is transferred to the `OUT` adapter. |
| Confirming | If the output value has changed, the `CNF` event is emitted. The FB then returns to the idle state. |

## Application Scenarios

- Selecting one of six AUI data sources based on a dynamic index.
- Routing adapter-based signals in modular automation systems.
- Switching between measurement or configuration values without generating redundant events.
- Use in applications where event-on-change behavior is required to reduce communication load.

## Comparison with Similar Blocks

Compared to a conventional IEC 61499 MUX block, AUI_AUI_MUX_6 does not use individual data inputs and a `REQ` event input. Instead, it uses unidirectional AUI adapter sockets, which allows a more structured and reusable connection style.

In contrast to a standard multiplexer, this FB emits its confirmation event only when the output value actually changes. This makes it particularly useful in event-driven systems where unnecessary events should be avoided.

As a generic FB, it can be instantiated through the generic class name `GEN_AUI_AUI_MUX`, providing flexible tool support while keeping the adapter-based interface consistent.

## Conclusion

AUI_AUI_MUX_6 is a compact and useful building block for adapter-based selection and routing tasks. It combines multiplexing functionality with event-on-change behavior, reducing unnecessary output updates and confirming only actual value changes. Its six AUI inputs make it well suited for systems that need to choose among multiple unidirectional adapter connections.

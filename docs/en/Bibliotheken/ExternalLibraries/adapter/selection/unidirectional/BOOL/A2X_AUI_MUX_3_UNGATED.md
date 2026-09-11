# A2X_AUI_MUX_3_UNGATED

![A2X_AUI_MUX_3_UNGATED](./A2X_AUI_MUX_3_UNGATED.svg)

* * * * * * * * * *

## Introduction

A2X_AUI_MUX_3_UNGATED is a multiplexer function block from the A2X family. It selects one of three A2X input streams and forwards it to a single A2X output stream. The selection is controlled through an AUI index adapter.

The key property of this block is described by the "UNGATED" suffix: it does **not** perform any change detection. Every newly calculated result is passed on unconditionally, even if the value itself has not changed. This makes the block suitable for consumers that require a periodic data cadence, for example derivative or frequency calculations.

## Interface Structure

The block does not expose direct event inputs, data inputs, or data outputs. All input and output information is exchanged through adapter connections. The following subsets describe the FB interface as visible in the type definition.

### **Event Inputs**

None.

### **Event Outputs**

| Name | Description |
|------|-------------|
| `CNF` | Confirmation that the index `K` has been applied and the corresponding input is now selected. |

### **Data Inputs**

None at the FB boundary. Input data is carried via the adapter sockets.

### **Data Outputs**

None at the FB boundary. Output data is carried via the adapter plug.

### **Adapters**

| Role | Name | Type | Direction | Description |
|------|------|------|-----------|-------------|
| Output | `OUT` | `adapter::types::unidirectional::A2X` | Plug | Selected value stream. `IN1` for `K=0`, `IN2` for `K=1`, `IN3` for `K=2`. |
| Selection | `K` | `adapter::types::unidirectional::AUI` | Socket | Index input that determines which A2X input is routed to `OUT`. |
| Input 1 | `IN1` | `adapter::types::unidirectional::A2X` | Socket | Input value 1, selected when `K = 0`. |
| Input 2 | `IN2` | `adapter::types::unidirectional::A2X` | Socket | Input value 2, selected when `K = 1`. |
| Input 3 | `IN3` | `adapter::types::unidirectional::A2X` | Socket | Input value 3, selected when `K = 2`. |

## Functionality

The function block behaves as a three-way multiplexer:

1. It reads the current index from the `K` adapter.
2. According to the index value, it selects one of the three A2X input adapters.
3. It forwards the selected input stream to the `OUT` adapter.
4. It emits `CNF` to confirm that the new index has been applied.

The block is "ungated", meaning there is no internal comparison between the current value and the previously forwarded value. Every new calculation result from the selected input is forwarded without filtering. This is intentional and is the main difference from a change-detecting multiplexer.

## Technical Features

- Generic FB implementation based on the attribute `eclipse4diac::core::GenericClassName` with the value `GEN_A2X_AUI_MUX`.
- The block supports exactly three A2X input channels.
- The adapter types are defined in the package `adapter::types::unidirectional`.
- No direct data inputs or outputs; all data transfer occurs through unidirectional adapters.
- Single event output `CNF` for confirmation of the selection index.
- No value-change recognition, no gating logic, no event suppression.

## State Overview

The block is intentionally stateless with respect to value history. It does not store previous A2X values and does not compare incoming values with earlier ones. Once an index is supplied via `K`, the corresponding input is routed to `OUT` without additional conditions.

The only implicit state is the currently active selection index. After a new index is received and processed, `CNF` is emitted and the new selection becomes active.

## Application Scenarios

Typical use cases include:

- Periodic distribution of measured or calculated values to downstream blocks that must be triggered on every scan, regardless of whether the value changed.
- Derivative and frequency calculations that need a continuous data stream.
- Data routing in automation systems where multiple A2X sources must be cyclically switched to one consumer.
- Applications that require a steady cadence for timing-sensitive calculations.

## Comparison with Similar Blocks

The closest relative is `A2X_AUI_MUX_3`. Both blocks implement the same three-input multiplexer structure. The difference is the presence of change detection:

- `A2X_AUI_MUX_3` suppresses forwarding when the value has not changed.
- `A2X_AUI_MUX_3_UNGATED` forwards every new result unconditionally.

For consumers that only need to react to actual value changes, the gated variant is more efficient. For consumers that require a periodic processing rhythm, the ungated variant is the better choice because it preserves the data cadence.

## Conclusion

A2X_AUI_MUX_3_UNGATED is a useful multiplexer block for unidirectional A2X data streams. By omitting change detection, it guarantees that every incoming result is passed through, making it particularly suitable for applications with periodic timing requirements. Its interface is compact and based entirely on adapters, allowing clean integration into 4diac IEC 61499 systems.

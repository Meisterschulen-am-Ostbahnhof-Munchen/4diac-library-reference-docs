# A2X_AUI_MUX_4_UNGATED

![A2X_AUI_MUX_4_UNGATED](./A2X_AUI_MUX_4_UNGATED.svg)

* * * * * * * * * *

## Introduction

A2X_AUI_MUX_4_UNGATED is a unidirectional, adapter-based 4-to-1 multiplexer for A2X adapter streams. It selects one of four A2X input adapters according to an index received through an AUI adapter and forwards the selected stream to an A2X output plug.

In contrast to a change-detecting multiplexer, the “UNGATED” variant does not compare old and new values. Every newly calculated result is forwarded unconditionally. This makes the block especially suitable for consumers that require a periodic or continuous sample flow, such as derivative or frequency calculations.

* * * * * * * * * *

## Interface Structure

The FB has no direct data inputs or data outputs. All runtime values are transported through unidirectional adapters. The interface consists of one event output, one index socket, four input sockets, and one output plug.

### **Event Inputs**

There are no explicit event inputs. Control and data flow are handled through the adapter connections.

### **Event Outputs**

| Event | Description |
|-------|-------------|
| `CNF` | Confirmation that the index `K` has been applied and the selected input has been forwarded to the output. |

### **Data Inputs**

None. All input values are carried by the adapter sockets.

### **Data Outputs**

None. The selected A2X value is provided through the `OUT` adapter plug.

### **Adapters**

| Adapter | Type | Direction | Description |
|---------|------|-----------|-------------|
| `K`    | `adapter::types::unidirectional::AUI` | Socket | Selection index. |
| `IN1`  | `adapter::types::unidirectional::A2X` | Socket | Input value 1, selected when `K = 0`. |
| `IN2`  | `adapter::types::unidirectional::A2X` | Socket | Input value 2, selected when `K = 1`. |
| `IN3`  | `adapter::types::unidirectional::A2X` | Socket | Input value 3, selected when `K = 2`. |
| `IN4`  | `adapter::types::unidirectional::A2X` | Socket | Input value 4, selected when `K = 3`. |
| `OUT`  | `adapter::types::unidirectional::A2X` | Plug   | Selected A2X output stream. |

* * * * * * * * * *

## Functionality

A2X_AUI_MUX_4_UNGATED acts as a four-channel multiplexer for A2X adapter data. The `K` socket carries the multiplexer index. This index is interpreted as an integer value in the range `0` to `3`.

The input selection is mapped as follows:

- `K = 0` → `IN1`
- `K = 1` → `IN2`
- `K = 2` → `IN3`
- `K = 3` → `IN4`

The selected input adapter is connected to the `OUT` plug, and the data is forwarded without any value-change filtering. After the selection and forwarding step has been completed, the event output `CNF` is issued as a confirmation.

The main characteristic of this FB is that it is “ungated”. A normal gated multiplexer may suppress an output update when the selected value has not changed. This block does not do that. Every updated result coming from the selected input is passed through to `OUT`, even if it is identical to the previously forwarded value.

* * * * * * * * * *

## Technical Features

- Generic FB implementation class: `GEN_A2X_AUI_MUX`
- Adapter-based interface: no conventional data inputs or outputs
- Unidirectional adapter types from the package `adapter::types::unidirectional`
- Four A2X input channels plus one AUI index channel
- One A2X output plug
- Single confirmation event output: `CNF`
- No change detection or value filtering
- Suitable for periodic, time-driven data consumers

* * * * * * * * * *

## State Overview

The FB type is declared as a generic FB, so the XML does not contain an explicit execution control chart. The actual behavior is provided by the generic implementation `GEN_A2X_AUI_MUX`. Conceptually, the block operates in the following cycle:

| State | Description |
|-------|-------------|
| Waiting | The FB waits for an update on the `K` index adapter or on the selected A2X input. |
| Decoding | The index `K` is evaluated and mapped to `IN1`, `IN2`, `IN3`, or `IN4`. |
| Forwarding | The selected input data is passed to the `OUT` plug. |
| Confirmation | The `CNF` event is emitted to confirm that the output has been updated. |

No state compares current and previous values. Therefore, every processed result is forwarded unconditionally.

* * * * * * * * * *

## Application Scenarios

- **Periodic derivative calculations**  
  Derivative blocks need a constant sample rhythm. If a value remains unchanged and a change-detecting multiplexer blocks the update, the derivative calculation may miss a sample interval. This UB forwards every new result and preserves the periodic cadence.

- **Frequency or rate measurement**  
  Frequency and rate calculations rely on time-based updates rather than value changes. The ungated behavior keeps the downstream calculation active.

- **Selecting among four A2X sources**  
  The FB can switch between four independent A2X adapter streams while maintaining a single, continuous output stream.

- **Adapter-based signal routing**  
  Because the FB uses only adapters, it can be integrated into unidirectional, adapter-oriented data flows without mixing in conventional data inputs or outputs.

- **Synchronization and handshaking**  
  The `CNF` output can be used by downstream logic to know that the selected input has been accepted and the output has been updated.

* * * * * * * * * *

## Comparison with Similar Blocks

| Block | Behavior |
|-------|----------|
| `A2X_AUI_MUX_4` | A four-input A2X multiplexer with change detection. It may suppress forwarding when the selected input value has not changed. |
| `A2X_AUI_MUX_4_UNGATED` | Same multiplexing function, but without change detection. Every newly calculated result is forwarded unconditionally. |
| `AX_AUI_MUX_4_UNGATED` | A non-A2X variant of the same ungated selection concept. The A2X variant uses dedicated unidirectional A2X adapter types. |

The essential difference is the “UNGATED” behavior: downstream blocks that require uniform sample timing should use this variant instead of a change-detecting multiplexer.

* * * * * * * * * *

## Conclusion

A2X_AUI_MUX_4_UNGATED is a specialized adapter-based multiplexer that selects one of four A2X inputs and forwards the selected data without change detection. Its strength is deterministic, periodic forwarding of all calculated results, which makes it ideal for time-critical consumers such as derivative and frequency calculations.

By combining a clear four-input structure, an AUI-based index interface, and a confirmation output, the block offers a simple yet flexible building block for unidirectional adapter-oriented control and measurement applications.

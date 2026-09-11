# A2X_AUI_MUX_5_UNGATED

![A2X_AUI_MUX_5_UNGATED](./A2X_AUI_MUX_5_UNGATED.svg)

* * * * * * * * * *

## Introduction

The **A2X_AUI_MUX_5_UNGATED** is an adapter-based, unidirectional 5-to-1 multiplexer. It selects one of five `A2X` input adapters through an `AUI` index adapter and forwards the selected value to an `A2X` output adapter.  
The `_UNGATED` variant does **not** perform change detection: every newly computed result is forwarded unconditionally. This makes it suitable for consumers that require a periodic output cadence independent of whether the actual value has changed, for example derivative or frequency calculations.

## Interface Structure

### **Event Inputs**

None.

The FB is triggered through its adapter sockets rather than through explicit event inputs.

### **Event Outputs**

| Name | Type | Comment |
|---|---|---|
| `CNF` | Event | Confirmation that the selected index `K` has been processed and the output has been updated. |

### **Data Inputs**

None.

All data is transported through the adapter sockets.

### **Data Outputs**

None.

All data is transported through the adapter plug.

### **Adapters**

| Name | Type | Direction | Comment |
|---|---|---|---|
| `OUT` | `adapter::types::unidirectional::A2X` | Plug | Selected input value: `IN1` for `K = 0`, `IN2` for `K = 1`, `IN3` for `K = 2`, `IN4` for `K = 3`, `IN5` for `K = 4`. |
| `K` | `adapter::types::unidirectional::AUI` | Socket | Index selector. |
| `IN1` | `adapter::types::unidirectional::A2X` | Socket | Input value 1, selected when `K = 0`. |
| `IN2` | `adapter::types::unidirectional::A2X` | Socket | Input value 2, selected when `K = 1`. |
| `IN3` | `adapter::types::unidirectional::A2X` | Socket | Input value 3, selected when `K = 2`. |
| `IN4` | `adapter::types::unidirectional::A2X` | Socket | Input value 4, selected when `K = 3`. |
| `IN5` | `adapter::types::unidirectional::A2X` | Socket | Input value 5, selected when `K = 4`. |

## Functionality

The FB works as a unidirectional adapter multiplexer:

1. The index adapter `K` determines which of the five input adapters is currently selected.
2. The selected input adapter is connected through to the output adapter `OUT`.
3. Every newly available result on the selected input is forwarded immediately and unconditionally.
4. The event output `CNF` is emitted as confirmation that the forwarding step has been completed.

The selection mapping is zero-based:

| `K` | Selected Input |
|---|---|
| `0` | `IN1` |
| `1` | `IN2` |
| `2` | `IN3` |
| `3` | `IN4` |
| `4` | `IN5` |

Because the FB is **ungated**, it does not compare old and new values and does not suppress repeated or unchanged data. Any refresh of the selected input results in an output update and a `CNF` event.

## Technical Features

- Adapter-only interface with no direct data inputs or outputs.
- Unidirectional data flow from input sockets to the output plug.
- 5 input channels and 1 output channel.
- Zero-based channel selection via the `AUI` index adapter.
- No change detection, no value filtering, no hysteresis.
- Emits exactly one `CNF` event per forwarded result.
- Configured as a generic FB type using `GEN_A2X_AUI_MUX`.
- Belongs to the compiler package `adapter::selection::unidirectional`.

## State Overview

The FB does not define an explicit internal state machine. Its behavior is essentially stateless and transparent:

| Aspect | Description |
|---|---|
| Current selection | Determined by the current value delivered through the `K` adapter. |
| Forwarding | The selected `INx` adapter is continuously passed through to `OUT`. |
| Confirmation | `CNF` is issued after each forwarding operation. |
| Change detection | Not present. Every newly computed result is forwarded. |

There is no stored "last value" and no internal comparison logic.

## Application Scenarios

Typical use cases for `A2X_AUI_MUX_5_UNGATED` include:

- Periodic derivative calculations that require an input sample on every cycle, even if the value is unchanged.
- Frequency or rate-of-change computations that depend on a constant time base.
- Data acquisition systems selecting between multiple sensors while maintaining a regular output rhythm.
- Pipelines where downstream blocks expect a fixed update cadence rather than change-triggered events.
- Adapter-based unidirectional communication paths that need a simple 5-to-1 selection mechanism.

## Comparison with Similar Blocks

The closest counterpart is the change-detecting `A2X_AUI_MUX_5` variant.

| Feature | `A2X_AUI_MUX_5` | `A2X_AUI_MUX_5_UNGATED` |
|---|---|---|
| Change detection | Yes | No |
| Forwarding behavior | Only forwards when the value has changed | Forwards every newly computed result |
| Output cadence | Event-driven / aperiodic | Periodic cadence possible |
| Best suited for | Consumers that react only to value changes | Consumers that need a continuous or periodic stream of results |
| CPU load on unchanged data | Lower | Higher, because every result is passed through |

Thus, the ungated variant is not a replacement for the gated variant, but a complementary block for time-oriented applications.

## Conclusion

`A2X_AUI_MUX_5_UNGATED` is a compact, adapter-only multiplexer for selecting one of five unidirectional `A2X` inputs via an `AUI` index. Its main characteristic is the unconditional forwarding of every newly computed result, independent of value changes. This behavior makes it well suited for periodic, time-sensitive consumers, especially derivative and frequency calculations, while still providing the familiar multiplexer structure and a clear `CNF` confirmation event.

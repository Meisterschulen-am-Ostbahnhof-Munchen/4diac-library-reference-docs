# AX_SPLIT_TO_2x_AX_TP


![AX_SPLIT_TO_2x_AX_TP_network](./AX_SPLIT_TO_2x_AX_TP_network.svg)

![AX_SPLIT_TO_2x_AX_TP](./AX_SPLIT_TO_2x_AX_TP.svg)

* * * * * * * * * *

## Introduction

This subapplication provides a generic solution for splitting a single AX (axis) event input into two independent retriggerable pulse timers. Each timer has its own adjustable pulse duration and produces a separate AX output. The subapp is hardware-independent and can be reused in various control scenarios.

## Interface Structure

### **Event Inputs**

There are no direct event inputs. However, the adapter `IN` acts as an event-triggering input; events received on this adapter are processed internally.

### **Event Outputs**

There are no direct event outputs. The generated pulse events are exposed via the adapter outputs `Q1` and `Q2`.

### **Data Inputs**

| Name | Type | Description |
|------|------|-------------|
| `TQ1` | `TIME` | Pulse duration for the first output (Q1). |
| `TQ2` | `TIME` | Pulse duration for the second output (Q2). |

### **Data Outputs**

None.

### **Adapters**

| Name | Type | Direction | Description |
|------|------|-----------|-------------|
| `IN` | `adapter::types::unidirectional::AX` | Input (Socket) | Receives the incoming AX event. |
| `Q1` | `adapter::types::unidirectional::AX` | Output (Plug) | Emits the pulse event for the first channel. |
| `Q2` | `adapter::types::unidirectional::AX` | Output (Plug) | Emits the pulse event for the second channel. |

## Functionality

The subapp uses an internal `AX_SPLIT_2` instance to split the incoming AX event into two identical AX events. These events are then forwarded to two separate `AX_TP` timer instances (`AX_TP_Q1` and `AX_TP_Q2`). Each timer is retriggerable: every new incoming event restarts the timing period. The pulse duration is independently set by `TQ1` and `TQ2`. When a timing period elapses, the corresponding timer outputs an AX event on its `Q` adapter, which is then relayed to the respective subapp output (`Q1` or `Q2`).

The split operation is transparent, meaning both channels receive the same trigger event. The timers operate independently, allowing different pulse widths and asynchronous behavior.

## Technical Features

- **Generic and hardware-independent**: The subapp does not rely on any specific hardware platform.
- **Two independent retriggerable timers**: Each channel has its own timer and pulse duration.
- **Split of input event**: Uses the `AX_SPLIT_2` block to duplicate the incoming event.
- **Easy configuration**: Pulse durations are set via data inputs.
- **Standard 61499 compliance**: Uses standard adapter types (unidirectional AX) and timers.

## State Overview

The subapp does not maintain an explicit state machine; its behavior is determined by the internal `AX_TP` timers. The timers operate in a retriggerable manner: upon receiving an input event, they start (or restart) their timing cycle. When the timing period elapses, they emit an output event. While the timer is active, further input events reset the timing.

## Application Scenarios

- **Control of two independent actuators** based on a single trigger signal.
- **Generation of two different pulse widths** for testing or calibration purposes.
- **Staggered pulse outputs** where each channel has its own response delay or duration.
- **Event distribution in automation systems** where a single sensor event must trigger multiple devices with different on-times.

## Comparison with Similar Blocks

Unlike a fixed pulse generator or a single retriggerable timer, this subapp provides dual-channel capability with independent timing settings. It can be seen as a combination of an event splitter and two pulse timers, offering flexibility not found in single-timer blocks. Compared to using two separate `AX_TP` instances manually, this subapp encapsulates the split and timer logic, reducing wiring complexity and improving readability.

## Conclusion

`AX_SPLIT_TO_2x_AX_TP` is a compact, reusable component for scenarios requiring an AX trigger to be duplicated and delayed with independent pulse durations. Its clear interface, generic design, and straightforward configuration make it a valuable addition to any 4diac-based automation project.
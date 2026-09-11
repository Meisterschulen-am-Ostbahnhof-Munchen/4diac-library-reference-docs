# T_FF_EVENT


![T_FF_EVENT_network](./T_FF_EVENT_network.svg)

![T_FF_EVENT](./T_FF_EVENT.svg)

* * * * * * * * * *
## Introduction

T_FF_EVENT is a generic toggle flip-flop subapplication implemented by combining an `E_SWITCH` and an `E_SR` function block. It provides a simple, event-driven mechanism where every activation of the `IND` event input toggles the boolean output `Q`. A corresponding `EO` event is emitted whenever the state changes, making the block suitable for edge detection and state-based control logic. The subapplication is fully generic and does not depend on any specific hardware platform.

## Interface Structure

### **Event Inputs**

| Name | Type | Description |
|------|------|-------------|
| `IND` | Event | Input event that triggers the toggle operation. Each occurrence of this event flips the internal state and updates `Q`. |

### **Event Outputs**

| Name | Type | Description |
|------|------|-------------|
| `EO` | Event | Output event fired after the internal state has changed. It signals that a toggle has occurred and `Q` has been updated. |

### **Data Inputs**

No data inputs are available for this subapplication.

### **Data Outputs**

| Name | Type | Description |
|------|------|-------------|
| `Q` | BOOL | Current state of the toggle flip-flop. It alternates between `FALSE` and `TRUE` with each `IND` event. |

### **Adapters**

No adapters are present in this subapplication.

## Functionality

The internal network consists of an `E_SWITCH` block (named `E_SWITCH_I1`) and an `E_SR` block (named `E_SR_I1`), interconnected as follows:

1. The incoming `IND` event is routed to the `EI` input of `E_SWITCH_I1`.
2. The `G` (gate) input of `E_SWITCH_I1` is driven by the current output `Q` of `E_SR_I1`.
3. When `IND` occurs, `E_SWITCH_I1` evaluates its gate:
   - If `G = FALSE` (i.e., `Q = FALSE`), it issues an event on output `EO0`, which is connected to the `S` (set) input of `E_SR_I1`. This sets `Q` to `TRUE`.
   - If `G = TRUE` (i.e., `Q = TRUE`), it issues an event on output `EO1`, which is connected to the `R` (reset) input of `E_SR_I1`. This resets `Q` to `FALSE`.
4. After the state change, `E_SR_I1` emits its `EO` output event, which is forwarded to the subapplication's `EO` output.

Thus, every `IND` event flips the state of `Q`, and the `EO` event confirms that the toggle has been completed.

## Technical Features

- **Event-driven toggle**: The flip-flop changes state only when an `IND` event is received, not on a continuous level signal.
- **Generic implementation**: Built from standard IEC 61499 function blocks (`E_SWITCH` and `E_SR`), which are part of the basic event function block library.
- **No hardware dependency**: The logic is purely algorithmic and can be applied in any IEC 61499 runtime environment.
- **Single output**: The only data output is the boolean state `Q`, while the `EO` event provides a change notification.
- **No data inputs**: The subapplication requires no configuration parameters or external data to operate.

## State Overview

The toggle flip-flop has two stable states, determined by the output `Q`:

| State | `Q` Value | Description |
|-------|-----------|-------------|
| OFF (Reset) | `FALSE` | Initial state. The next `IND` event will set `Q` to `TRUE`. |
| ON (Set) | `TRUE` | Toggled state. The next `IND` event will reset `Q` to `FALSE`. |

The transition between states is strictly sequential: each `IND` event moves the block from one state to the other, and `EO` is emitted on every transition. There is no hold, debounce, or delay logic inside the block; the toggle is immediate and synchronous with the input event.

## Application Scenarios

T_FF_EVENT is well suited for a variety of use cases where a binary state must be toggled by an event signal:

- **Push-button toggling**: Turning a light or actuator on and off with a momentary pushbutton, where each press generates an `IND` event.
- **Mode switching**: Alternating between two operating modes (e.g., automatic/manual) in a control application.
- **Edge detection and reporting**: Using `EO` to notify other parts of the system that the state has changed, e.g., to trigger logging or further processing.
- **State-based sequencing**: As a building block in larger subapplications where a binary flag needs to be inverted under event control.

Because the block is generic and hardware-independent, it can be reused across different projects, from simple logic exercises to industrial control systems.

## Comparison with Similar Blocks

- **E_SR / E_RS (Set-Reset flip-flops)**: These blocks require two separate event inputs (`S` and `R`, or `R` and `S`) to set or reset the output. T_FF_EVENT is different because it requires only a single event input and automatically alternates between set and reset. It is equivalent to an E_SR with its set/reset inputs fed from an E_SWITCH controlled by its own output.

- **SR (data-level flip-flop)**: A standard SR latch uses level signals rather than events. T_FF_EVENT is event-driven and has no data input, so it cannot be forced to a particular state from outside; it always toggles from its current state.

- **T-flip-flop (binary counter)**: A classic T-flip-flop toggles on each active clock edge. T_FF_EVENT implements exactly this behavior using IEC 61499 events, but without a separate clock input; the `IND` event itself acts as the toggle trigger.

## Conclusion

T_FF_EVENT is a compact and reusable subapplication that provides a simple toggle behavior with an event output for change notification. By combining an `E_SWITCH` and an `E_SR` in a feedback configuration, it achieves a clean, edge-triggered toggle flip-flop without external dependencies. Its generic design, minimal interface, and clearly defined state behavior make it an ideal building block for event-driven control systems, batch processing, and state management applications.
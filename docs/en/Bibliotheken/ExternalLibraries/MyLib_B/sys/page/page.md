# page


![page_network](./page_network.svg)

![page](./page.svg)

* * * * * * * * * *

## Introduction

The `page` subapplication is a compact IEC 61499 composite type that encapsulates an ISOBUS status monitoring block together with an event-triggered D flip-flop. It exposes a single event output `CNF` and provides no external data interface. Internally, the subapplication uses the `CbVtStatus` block from the ISOBUS UT status library to detect a status indication and the standard `E_D_FF` block to sample a boolean value at exactly the moment the indication occurs.

## Interface Structure

The subapplication interface is intentionally minimal: it only exposes one event output. All data and event processing is done inside the encapsulated network.

### **Event Inputs**

- None

### **Event Outputs**

- `CNF` : Execution Confirmation  
  This event is emitted after the internal event chain has been completed. It confirms that the status indication has been processed and the associated boolean value has been latched.

### **Data Inputs**

- None

### **Data Outputs**

- None

### **Adapters**

- None

## Functionality

Internally, the subapplication contains two function blocks:

1. `CbVtStatus` of type `isobus::UT::status::CbVtStatus`  
   This block monitors an ISOBUS UT status condition. When a status indication occurs, it produces the event `IND`. It also provides the boolean data output `qWsActive`, which represents the current working-set-active state.

2. `E_D_FF` of type `iec61499::events::E_D_FF`  
   This is an event-triggered D flip-flop. It uses the event input `CLK` to sample the boolean value present at its data input `D`. When the clock event occurs, the flip-flop latches the value of `D` and emits the event output `EO`.

The internal connections perform the following task:

- `CbVtStatus.IND` is connected to `E_D_FF.CLK`, so every status indication triggers the flip-flop.
- `CbVtStatus.qWsActive` is connected to `E_D_FF.D`, so the current working-set-active value is used as the data input.
- `E_D_FF.EO` is connected to the subapplication output `CNF`, so the confirmation event is emitted after the value has been latched.

In effect, the subapplication captures the boolean status value exactly at the time of the indication and produces a confirmation event for the surrounding application.

## Technical Features

- Encapsulated subapplication with no exposed event or data inputs.
- Single event output `CNF` for handshake-based integration.
- Internal event flow: `CbVtStatus.IND` → `E_D_FF.CLK` → `E_D_FF.EO` → `CNF`.
- Internal data flow: `CbVtStatus.qWsActive` → `E_D_FF.D`.
- Uses standard IEC 61499 components from the `iec61499::events` library.
- Uses an ISOBUS-specific status block from the `isobus::UT::status` library.
- The externally visible behavior is event-driven and synchronous: one status indication results in one confirmation event.

## State Overview

The internal `E_D_FF` block can be considered as having two stable states:

| State | Latched Value | Description |
|-------|---------------|-------------|
| Reset | `false` | `qWsActive` was `false` at the last `CLK` event |
| Set   | `true`  | `qWsActive` was `true` at the last `CLK` event |

The state is held internally and is not exposed through the subapplication data interface. The only visible effect is that each `CLK` event causes the `EO` event to be emitted, which is forwarded as `CNF`.

## Application Scenarios

This subapplication is suitable for event-driven ISOBUS applications where the controller must be informed whenever a VT status indication occurs. Typical use cases include:

- Monitoring the working-set-active status of an ISOBUS Universal Terminal.
- Capturing a boolean status value at a precise event boundary.
- Generating a confirmation event for use in a larger function block network.
- Synchronizing status evaluation with downstream event-triggered logic.

Because the subapplication exposes only a confirmation event, it can be integrated cleanly into systems where data transfer is not required or where the status value is evaluated further inside another composite block.

## Comparison with Similar Blocks

Compared with a simple direct connection from `CbVtStatus.IND` to the subapplication output, this subapplication additionally samples and latches the `qWsActive` data value. That makes the captured state stable and independent of later changes to `qWsActive`.

Compared with an `E_RS_FF`-based solution, the `E_D_FF` approach uses a data input to define the next state instead of separate set and reset event paths. This is better suited for situations where the state should follow a sampled boolean signal.

Compared with a pure combinatorial event-forwarding block, the internal flip-flop ensures that the data value is consistent with the event that triggered the confirmation.

## Conclusion

The `page` subapplication is a small but useful reusable component. It combines ISOBUS VT status indication with a D-type event latch to provide a single confirmation event. Its minimal interface makes it easy to embed in larger 4diac applications, while the internal flip-flop guarantees that the associated boolean value is captured at the correct moment.

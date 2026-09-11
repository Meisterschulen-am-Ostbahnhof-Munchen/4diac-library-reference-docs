# ILOCK_SWITCH_A2X


![ILOCK_SWITCH_A2X_ecc](./ILOCK_SWITCH_A2X_ecc.svg)

![ILOCK_SWITCH_A2X](./ILOCK_SWITCH_A2X.svg)

* * * * * * * * * *

## Introduction

The `ILOCK_SWITCH_A2X` is an event-driven basic function block that implements a two-directional interlock switch for an A2X adapter signal. It accepts UP/DOWN requests through an `IN` adapter and forwards the interlocked result through an `OUT` adapter. Only one direction can be active at a time. If both directions are requested, the last active input wins.

## Interface Structure

The FB does not expose ordinary IEC 61499 event or data inputs/outputs. All control information is exchanged through two A2X adapter instances.

### **Event Inputs**

None at the FB interface.

The FB receives events through the `IN` adapter:

- `IN.E_UP` — up-direction event, evaluated together with `IN.UP`.
- `IN.E_DOWN` — down-direction event, evaluated together with `IN.DOWN`.

### **Event Outputs**

None at the FB interface.

The FB sends events through the `OUT` adapter:

- `OUT.E_UP` — up-direction output event, accompanied by `OUT.UP`.
- `OUT.E_DOWN` — down-direction output event, accompanied by `OUT.DOWN`.

### **Data Inputs**

None at the FB interface.

The `IN` adapter supplies the boolean data:

- `IN.UP` — indicates whether the up direction is requested.
- `IN.DOWN` — indicates whether the down direction is requested.

### **Data Outputs**

None at the FB interface.

The `OUT` adapter supplies the boolean data:

- `OUT.UP` — `TRUE` while the up direction is active.
- `OUT.DOWN` — `TRUE` while the down direction is active.

### **Adapters**

| Adapter | Direction | Type | Description |
|---|---|---|---|
| `IN` | Socket | `adapter::types::unidirectional::A2X` | Input for forward/up and backward/down direction. |
| `OUT` | Plug | `adapter::types::unidirectional::A2X` | Output for forward/up and backward/down direction. |

## Functionality

The FB is a basic FB controlled by an ECC. It evaluates the A2X input events and data and produces an interlocked A2X output.

- In the `STOP` state, both `OUT.UP` and `OUT.DOWN` are `FALSE`; no direction is active.
- If an `IN.E_UP` event arrives with `IN.UP = TRUE`, the FB enters `UP` and activates `OUT.UP`.
- If an `IN.E_DOWN` event arrives with `IN.DOWN = TRUE`, the FB enters `DOWN` and activates `OUT.DOWN`.
- If a direction event arrives that requests the other direction while one direction is active, the FB switches to the other direction. This ensures last-active-input priority.
- If the active direction is released and no other direction is requested, the FB passes through a stop state and returns to `STOP`.
- If the active direction is released but the other direction is still requested, the FB switches directly to the remaining direction.

The algorithms used by the states are:

| Algorithm | `OUT.UP` | `OUT.DOWN` |
|---|---|---|
| `UP` | `TRUE` | `FALSE` |
| `DOWN` | `FALSE` | `TRUE` |
| `STOP` | `FALSE` | `FALSE` |

The output events are generated together with these data values, so a receiving block can react both to a direction change and to the release of the opposite direction.

## Technical Features

- IEC 61499 basic FB with ECC-based, event-driven behavior.
- No external event/data inputs or outputs; all signals are transported by the unidirectional A2X adapters.
- Interlocked output: `OUT.UP` and `OUT.DOWN` can never be `TRUE` at the same time.
- Last-event priority when both input directions are active.
- Automatic stop handling through transient `UP_STOP` and `DOWN_STOP` states.
- Suitable for integration in `logiBUS::signalprocessing::interlock` signal-processing chains.

## State Overview

| State | Output Data | Output Events | Meaning |
|---|---|---|---|
| `STOP` | `OUT.UP = FALSE`, `OUT.DOWN = FALSE` | — | No direction active. |
| `UP` | `OUT.UP = TRUE`, `OUT.DOWN = FALSE` | `OUT.E_UP`, `OUT.E_DOWN` | Up direction active. |
| `DOWN` | `OUT.UP = FALSE`, `OUT.DOWN = TRUE` | `OUT.E_DOWN`, `OUT.E_UP` | Down direction active. |
| `UP_STOP` | `OUT.UP = FALSE`, `OUT.DOWN = FALSE` | `OUT.E_UP` | Transient stop from up direction. |
| `DOWN_STOP` | `OUT.UP = FALSE`, `OUT.DOWN = FALSE` | `OUT.E_DOWN` | Transient stop from down direction. |

The ECC transitions are:

| From | Condition | To |
|---|---|---|
| `STOP` | `IN.E_UP[IN.UP]` | `UP` |
| `STOP` | `IN.E_DOWN[IN.DOWN]` | `DOWN` |
| `UP` | `IN.E_DOWN[IN.DOWN]` | `DOWN` |
| `UP` | `IN.E_UP[NOT IN.UP AND IN.DOWN]` | `DOWN` |
| `UP` | `IN.E_UP[NOT IN.UP AND NOT IN.DOWN]` | `UP_STOP` |
| `DOWN` | `IN.E_UP[IN.UP]` | `UP` |
| `DOWN` | `IN.E_DOWN[NOT IN.DOWN AND IN.UP]` | `UP` |
| `DOWN` | `IN.E_DOWN[NOT IN.DOWN AND NOT IN.UP]` | `DOWN_STOP` |
| `UP_STOP` | `1` | `STOP` |
| `DOWN_STOP` | `1` | `STOP` |

## Application Scenarios

- Direction control of hydraulic or electric actuators that must not run in two directions simultaneously.
- Interlocking of forward/backward or up/down control signals on a logiBUS A2X connection.
- Conversion of a raw A2X input into a clean, interlocked A2X output for downstream control logic.
- Use as an adapter-based replacement for a conventional two-input interlock FB where UP/DOWN signals are fed from a single A2X input/output.

## Comparison with Similar Blocks

- Compared with a non-adapter `ILOCK_SWITCH`, this variant uses an A2X adapter for both input and output. This simplifies wiring when the surrounding system already uses A2X signals.
- Compared with an SR latch, this FB provides two separate direction outputs with associated events and an explicit stop state. It does not use a fixed set/reset priority; instead, the last active input wins.
- Compared with a fixed-priority direction selector, this FB can react to event/data combinations that indicate "one direction released, other still active" and switch to the remaining direction.

## Conclusion

`ILOCK_SWITCH_A2X` is a compact, adapter-based interlock element for two-direction control signals. It combines a simple state machine with A2X adapter input/output, provides last-active-input prioritization, and guarantees that the output never activates both directions at the same time. It is especially useful in logiBUS signal-processing applications where UP/DOWN commands are transported over a single A2X connection.
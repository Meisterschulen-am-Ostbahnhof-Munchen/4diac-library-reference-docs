# ILOCK_SWITCH_PROTECT_A2X


![ILOCK_SWITCH_PROTECT_A2X_ecc](./ILOCK_SWITCH_PROTECT_A2X_ecc.svg)

![ILOCK_SWITCH_PROTECT_A2X](./ILOCK_SWITCH_PROTECT_A2X.svg)

* * * * * * * * * *
## Introduction

The **ILOCK_SWITCH_PROTECT_A2X** function block is an adapter-based implementation of a protective interlock switch. It takes forward/up and backward/down commands via a single `A2X` input adapter, prioritizes the most recent active command, and applies a configurable dead-time (`DT_PROTECT`) before actually switching the output direction. This prevents rapid or accidental direction changes that could damage mechanical systems.

The block is designed for applications where a controlled directional actuation (e.g., a motor, valve, or shutter) must be protected against abrupt reversals. It uses an internal state machine to manage the protective delay and outputs the resulting direction via an `A2X` output adapter.

## Interface Structure

### **Event Inputs**

| Event | With | Comment |
|-------|------|---------|
| `UPDATE` | `DT_PROTECT` | Updates the protection dead-time parameter. The new value is taken from the associated data input. |

### **Event Outputs**

The block does not have direct event outputs. Instead, events are emitted through its output adapters:

- `OUT.E_UP` – signals the active upward/forward direction (via `OUT` adapter).
- `OUT.E_DOWN` – signals the active downward/backward direction (via `OUT` adapter).
- `timeOut.START` – starts the protective timer (via `timeOut` adapter).

### **Data Inputs**

| Name | Type | Initial Value | Comment |
|------|------|---------------|---------|
| `DT_PROTECT` | `TIME` | `T#50ms` | Protection dead-time, i.e., the minimum delay between a command change and the actual output transition. |

### **Data Outputs**

The block does not have direct data outputs. Direction data is provided via the `OUT` adapter:

- `OUT.UP` – boolean indicating upward/forward direction is active.
- `OUT.DOWN` – boolean indicating downward/backward direction is active.

### **Adapters**

| Adapter | Type | Direction | Comment |
|---------|------|-----------|---------|
| `IN` | `adapter::types::unidirectional::A2X` | Socket (input) | Provides input commands: events `E_UP`/`E_DOWN` and data `UP`/`DOWN`. |
| `OUT` | `adapter::types::unidirectional::A2X` | Plug (output) | Outputs the resulting direction: events `E_UP`/`E_DOWN` and data `UP`/`DOWN`. |
| `timeOut` | `iec61499::events::ATimeOut` | Plug (output) | Timer adapter used for the protective dead-time. It receives a `DT` data value and a `START` event. The `TimeOut` event is used internally to trigger evaluation after the delay. |

## Functionality

The block operates as a directional interlock with a configurable protection delay. It continuously monitors the input adapter `IN` for two command types:

- `UP` (forward/upward)
- `DOWN` (backward/downward)

When a command is received and there is no current movement, the block sets the output direction immediately (after a short internal evaluation). However, if a direction change is requested while the opposite direction is already active, the block does **not** switch directly. Instead, it enters a `PROTECT` state, stops all output, and starts the protection timer with `DT_PROTECT`. Once the timer expires, the block evaluates the current input conditions and decides the next state:

- If only `UP` is active, it switches to the `UP` state.
- If only `DOWN` is active, it switches to the `DOWN` state.
- If neither is active, it returns to `STOP`.
- If both are active, it remains in `PROTECT` and restarts the timer (this ensures that the last active command eventually wins, but no direction is set while both are given).

The `UPDATE` event can be used anytime to change the dead-time value. The new value is applied immediately, even if a protection cycle is already running (the timer uses the updated `DT_PROTECT` value).

## Technical Features

- **Adapter-based I/O**: Uses standard `A2X` adapters for both input and output, making it easy to interface with other blocks and reuse in different contexts.
- **Configurable dead-time**: The `DT_PROTECT` parameter (via `UPDATE`) allows the user to adjust the minimum delay before a direction change takes effect.
- **Priority handling**: The block resolves simultaneous `UP` and `DOWN` commands by staying in protective mode until only one direction remains active.
- **Transparent state machine**: The internal ECC clearly defines the states and transitions, ensuring deterministic behavior.
- **Timer integration**: Uses a standard `ATimeOut` adapter for timing, which simplifies integration with other timing resources.

## State Overview

The internal ECC consists of five states:

| State | Description |
|-------|-------------|
| `STOP` | Idle state. No output direction is active. The block waits for an input command. |
| `UP`   | Upward/forward direction is active. Outputs `OUT.UP = TRUE` and `OUT.DOWN = FALSE`. Emits `OUT.E_UP`. |
| `DOWN` | Downward/backward direction is active. Outputs `OUT.UP = FALSE` and `OUT.DOWN = TRUE`. Emits `OUT.E_DOWN`. |
| `PROTECT` | Protection delay active. All outputs are cleared (`OUT.UP = FALSE`, `OUT.DOWN = FALSE`). The timer is started with `DT_PROTECT`. |
| `EVAL`  | Evaluation state after the delay. Determines the next direction based on current input values. |

Transitions:

- From `STOP` to `UP` if `IN.E_UP` occurs and `IN.UP` is true.
- From `STOP` to `DOWN` if `IN.E_DOWN` occurs and `IN.DOWN` is true.
- From `UP` to `PROTECT` if `IN.E_UP` occurs with `IN.UP` false (i.e., release) or `IN.E_DOWN` occurs with `IN.DOWN` true (request opposite direction).
- From `DOWN` to `PROTECT` if `IN.E_DOWN` occurs with `IN.DOWN` false or `IN.E_UP` occurs with `IN.UP` true.
- From `PROTECT` to `EVAL` when the timer expires (`timeOut.TimeOut`).
- From `EVAL` to `UP`, `DOWN`, `STOP`, or `PROTECT` based on the combination of `IN.UP` and `IN.DOWN`.
- From any state back to itself on `UPDATE` (parameters are refreshed without changing the operating state).

## Application Scenarios

The **ILOCK_SWITCH_PROTECT_A2X** block is suitable for any system that requires safe direction control with a mandatory dwell time between reversals. Typical examples include:

- **Electric motors** driving conveyor belts or winches where sudden reversal can cause mechanical stress.
- **Roller shutters, blinds, or gates** that must not change direction abruptly.
- **Valves or actuators** with inherent mechanical limits that require a short settling time.
- **Robotic axes** where a reversal must be delayed to avoid overcurrent or overshoot.
- **Pumps or fans** that should not be toggled rapidly.

The block is especially useful in safety-critical applications where a defined delay is mandated by regulations.

## Comparison with Similar Blocks

Compared to a simple interlock switch (that switches direction immediately), this block adds a **protective dead-time**. This feature is essential when:

- The driven load has significant inertia and can be damaged by rapid direction changes.
- The control system must prevent simultaneous activation of both directions (e.g., to avoid short-circuiting a bridge).
- The application requires a minimum delay for settling or safety reasons.

The adapter version (`A2X`) offers a clean interface that separates control commands from the actual output, making it more modular than a version with direct I/O ports. It also simplifies integration with other adapter-based systems. The use of a standard timer adapter (`ATimeOut`) allows the dead-time to be synchronized with global timing resources.

In contrast to blocks without a prioritization mechanism, this block handles the case where both `UP` and `DOWN` are given simultaneously by waiting in `PROTECT` until one command disappears. This ensures that the last active input eventually wins, preventing an undefined state.

## Conclusion

The **ILOCK_SWITCH_PROTECT_A2X** function block provides a robust and configurable solution for interlocked direction control with a protective delay. Its adapter-based interface makes it easy to integrate into larger IEC 61499 applications, while the internal state machine guarantees deterministic behavior under all input conditions. The ability to dynamically update the dead-time via the `UPDATE` event adds flexibility for changing operational requirements. This block is particularly suited for applications where mechanical or electrical protections demand a minimum time between direction reversals.
# AE2_ILOCK_T_FF_TO_AX


![AE2_ILOCK_T_FF_TO_AX_network](./AE2_ILOCK_T_FF_TO_AX_network.svg)

![AE2_ILOCK_T_FF_TO_AX](./AE2_ILOCK_T_FF_TO_AX.svg)

* * * * * * * * * *

## Introduction

`AE2_ILOCK_T_FF_TO_AX` is a reusable subapplication that implements a single link of a **mutually interlocked toggle flip‑flop chain**. It is designed to be used in a network where multiple instances are connected in series via standard bidirectional AE2 adapters. The subapp guarantees that at any time only one output in the entire chain is active. A click (event) on the local input toggles the state of that link, while simultaneously forcing all other links to become inactive through a propagating reset signal sent along the chain.

The block is generic and can be used for any number of participants. It provides an AX adapter as the output interface, making it compatible with unidirectional adapter consumers.

## Interface Structure

### **Event Inputs**

| Name | Type | Description |
|------|------|-------------|
| `IND` | Event | Toggle trigger. When a rising edge occurs, the internal state changes (set if currently inactive, reset if currently active), and the reset signal is sent to neighboring links. |

### **Event Outputs**

The subapplication has **no dedicated event output** interfaces. All communication with other links is performed via the AE2 adapters.

### **Data Inputs

There are **no direct data inputs** exposed to the user. The internal logic relies solely on the event input and the data carried by the AE2 adapters.

### **Data Outputs

There are **no direct data outputs**. The result is available via the `Q` adapter (AX type), which carries a boolean value.

### **Adapters**

| Name | Type | Direction | Purpose |
|------|------|-----------|---------|
| `SOCKET` | `AE2` (bidirectional) | Socket (input side) | Receives reset signals from the previous link in the chain. |
| `PLUG` | `AE2` (bidirectional) | Plug (output side) | Sends reset signals to the next link in the chain. |
| `Q` | `AX` (unidirectional) | Plug (output side) | Provides the current state as a boolean value (true = active, false = inactive). |

## Functionality

The subapp implements a **toggle flip‑flop with mutual exclusion**. Its internal network consists of:

- An `E_SR` set/reset flip‑flop storing the current state.
- An `E_SWITCH` that routes the incoming `IND` event based on the current state.
- Two bidirectional adapter‑conversion blocks (`AE2_EVENT_TO_E` and `AE2_E_TO_EVENT`) that translate between regular events and AE2 adapter events.
- A unidirectional converter (`AX_BOOL_TO_X`) that maps the internal boolean state to the output adapter `Q`.

**Operation:**

1. **Toggle action:** When an `IND` event arrives, `E_SWITCH` checks the current state `Q`.
   - If `Q = false` (inactive), the event is routed to the `S` (set) input of `E_SR`, causing the state to become active. Simultaneously, an event is sent to the `AE2_EVENT_TO_E` block, which emits a reset signal on the `PLUG` adapter.
   - If `Q = true` (active), the event is routed to the `R` (reset) input of `E_SR`, causing the state to become inactive. No reset signal is propagated in this case.

2. **Interlock propagation:** When a reset signal is received from the previous link via the `SOCKET` adapter, the `AE2_E_TO_EVENT` block converts it to a regular event. This event resets the local `E_SR` (forcing `Q` to false) and also sends a new event to `AE2_EVENT_TO_E`, which forwards the reset signal to the next link via `PLUG`. Thus the reset propagates down the chain, ensuring all other links become inactive.

3. **Output:** The internal state `Q` is directly fed to the `AX_BOOL_TO_X` converter, which produces the boolean value on the `Q` adapter.

The combination of the toggle action and the reset propagation guarantees that after any `IND` event, exactly one link in the chain is active – the link that received the event (if it was inactive) – and all others are reset.

## Technical Features

- **Modularity:** The subapp is designed as a self‑contained chain element. Multiple instances can be connected by linking `PLUG` of one to `SOCKET` of the next.
- **Synchronisation:** The AE2 adapter conversions handle event propagation with minimal delay, and the internal SR flip‑flop ensures state stability.
- **Scalability:** The chain length is unlimited; the propagation delay grows linearly with the number of elements, but the interlock logic remains correct.
- **Output compatibility:** The `AX` output allows direct connection to unidirectional consumers (e.g., indicators, control words) without additional conversion.
- **Event‑driven:** All operations are triggered by events; no cyclic polling is required.

## State Overview

The subapp has two distinct states:

| State | `Q` value | Description |
|-------|-----------|-------------|
| Inactive | `false` | The link is not active. An incoming `IND` event will toggle it to active and propagate a reset signal. |
| Active | `true` | The link is the only active one in the chain. An incoming `IND` event toggles it back to inactive. |

**Transitions:**
- **Inactive → Active:** Triggered by `IND` when `Q = false`.
- **Active → Inactive:** Triggered by `IND` when `Q = true`, or by a reset signal received from the chain (via `SOCKET`).

Internal reset signals do not change the state if the link is already inactive; they are simply forwarded.

## Application Scenarios

- **Exclusive mode selection:** A set of mutually exclusive operating modes in a machine. Each mode is represented by one subapp instance; clicking a button selects the corresponding mode and deactivates all others.
- **Priority‑free resource allocation:** A group of clients that must share a single resource. The first client that requests the resource gets it, and all others are automatically denied.
- **Single‑active workflow steps:** A sequence of steps in a production system where only one step may run at a time, with manual triggering to advance.

## Comparison with Similar Blocks

- **Standard Toggle Flip‑Flop (`E_T**):** A simple toggle changes state on every event but has no interlock capability. Multiple toggles can be active simultaneously.
- **Independent SR Latches:** Without a chain, each latch would need explicit external logic to enforce mutual exclusion. This subapp encapsulates the necessary interlock in a reusable and scalable manner.
- **Centralised Arbiter:** A central arbiter could be used, but it would require additional wiring and a different communication pattern. `AE2_ILOCK_T_FF_TO_AX` provides a decentralised solution with a simple daisy‑chain topology.

## Conclusion

`AE2_ILOCK_T_FF_TO_AX` is a compact and reusable subapplication that provides toggling behaviour with guaranteed mutual exclusion. Its chain‑based design automatically propagates resets, making it suitable for any number of participants without additional logic. The use of standard AE2 adapters ensures easy integration into IEC 61499 systems, and the AX output simplifies connection to ordinary boolean consumers. This block is ideal for scenarios where a single active state among many must be maintained with minimal wiring and high reliability.
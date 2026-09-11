# AX_E_PERMIT_2

![AX_E_PERMIT_2](./AX_E_PERMIT_2.svg)

* * * * * * * * * *

## Introduction

The AX_E_PERMIT_2 is a generic function block designed to propagate events from two independent input channels to corresponding output channels, but only when a permit condition is satisfied. The permit condition is provided externally via a unidirectional adapter of type `AX`. This block is useful for gating event flows based on dynamic or configurable criteria without hardcoding the logic inside the FB itself.

## Interface Structure

The block has no data inputs or outputs; it operates purely on event signals and an adapter-based condition input.

### **Event Inputs**

| Event   | Description                        |
|---------|------------------------------------|
| `EI1`   | Event input channel 1              |
| `EI2`   | Event input channel 2              |

### **Event Outputs**

| Event   | Description                        |
|---------|------------------------------------|
| `EO1`   | Event output channel 1             |
| `EO2`   | Event output channel 2             |

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

| Adapter | Type                                  | Description                           |
|---------|---------------------------------------|---------------------------------------|
| `PERMIT`| `adapter::types::unidirectional::AX`  | Unidirectional adapter providing the permit condition for both event channels. |

## Functionality

The AX_E_PERMIT_2 acts as a dual-channel event gate. When an event occurs on `EI1` or `EI2`, the block checks the current condition presented by the `PERMIT` adapter. If the condition evaluates to *permit* (i.e., the adapter indicates that events are allowed), the corresponding event is emitted on `EO1` or `EO2`, respectively. If the condition is not satisfied, the event is suppressed and no output is triggered.

The block is generic; the actual permit logic is implemented by the connected adapter instance. This allows the same FB to be reused in different contexts with different permit rules (e.g., based on sensor values, system state, or user-defined logic).

## Technical Features

- **Generic Design**: The FB is parameterized via the adapter type, enabling reusability across various permit scenarios.
- **Two Independent Channels**: EI1/EO1 and EI2/EO2 operate independently, each gated by the same adapter condition. This is useful for coordinating multiple event streams under a single permit criterion.
- **No Internal State**: The FB is stateless; it does not store information between event occurrences. It simply passes events through when permitted.
- **Unidirectional Adapter**: The `PERMIT` adapter is unidirectional, meaning the FB reads a value from the adapter without sending data back to the providing block.
- **Event Semantics**: All inputs and outputs are pure event signals, making this block suitable for event-driven control logic.

## State Overview

The FB maintains no internal state. Its behavior is deterministic and based solely on the current event input and the instantaneous value of the permit adapter. The block can be considered a combinatorial gate in the event domain.

## Application Scenarios

- **Safety Interlocking**: Allow event propagation only when a safety condition (e.g., machine guard closed) is met.
- **Mode-Dependent Operation**: Enable event flows only in certain operating modes of a system (e.g., automatic vs. manual).
- **Load Shedding**: Permit events only when system load is below a threshold.
- **Conditional Logging or Monitoring**: Forward events to a monitoring subsystem only when a specific state is active.

## Comparison with Similar Blocks

- **Direct Event Propagation (e.g., E_REND)**: Without a permit mechanism, events are always forwarded. AX_E_PERMIT_2 adds a gating condition.
- **Two Separate SINGLE-CHANNEL Permit Blocks**: Using two individual permit blocks would require duplicating the condition logic and potentially cause inconsistent states. AX_E_PERMIT_2 uses a single adapter for both channels, simplifying the design.
- **Event Combination Blocks (e.g., E_MERGE)**: These combine events but do not provide conditional filtering; AX_E_PERMIT_2 filters each channel independently.

## Conclusion

The AX_E_PERMIT_2 is a flexible, generic function block that efficiently manages event propagation under a shared permit condition. Its dual-channel structure, combined with an external adapter, makes it a valuable component for building event-driven control systems that require conditional gating. The stateless design ensures predictable behavior and easy integration into larger IEC 61499 orchestration.

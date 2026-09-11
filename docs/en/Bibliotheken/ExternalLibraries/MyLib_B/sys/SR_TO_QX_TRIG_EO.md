# SR_TO_QX_TRIG_EO


![SR_TO_QX_TRIG_EO_network](./SR_TO_QX_TRIG_EO_network.svg)

![SR_TO_QX_TRIG_EO](./SR_TO_QX_TRIG_EO.svg)

* * * * * * * * * *
## Introduction

The **SR_TO_QX_TRIG_EO** subapplication combines a set/reset latch (E_SR) with a digital output (logiBUS_QX) and a rising-edge trigger (E_R_TRIG). It provides a reusable, event-controlled digital output stage that emits an echo event (EO1) whenever the latched output state transitions from false to true. The block is designed as a generic solution: the target physical output (Q1…Q8) is selected via a dedicated data input.

## Interface Structure

### **Event Inputs**

| Name   | Description                                                                 |
|--------|-----------------------------------------------------------------------------|
| `SET`  | Event input that sets the internal latch, driving the output Q to `TRUE`.   |
| `RESET`| Event input that resets the internal latch, driving the output Q to `FALSE`.|

### **Event Outputs**

| Name | Description                                                                                     |
|------|-------------------------------------------------------------------------------------------------|
| `EO1`| Echo event emitted on the rising edge of the latch output (transition `FALSE` → `TRUE`).        |

### **Data Inputs**

| Name     | Type                        | Initial Value            | Description                                                        |
|----------|-----------------------------|--------------------------|--------------------------------------------------------------------|
| `Output` | `logiBUS::io::DQ::logiBUS_DO_S` | `logiBUS_DO::Invalid`    | Identifies the physical digital output to be driven (Output_Q1…Q8).|

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

The subapplication implements a classic latch-and-drive pattern:

1. On `SET`, the internal E_SR latch is set (`Q = TRUE`). On `RESET`, the latch is cleared (`Q = FALSE`).  
2. After every latch update (whether SET or RESET), the `Q` value is transferred to the logiBUS_QX digital output block, which writes the value to the physical output specified by the `Output` data input.  
3. The same `Q` signal is also fed to a rising-edge detector (E_R_TRIG). When `Q` changes from `FALSE` to `TRUE`, the detector fires the `EO1` event output.

Thus, `EO1` acts as an acknowledgment that the latch has been turned on, while the physical output is refreshed on every state change.

## Technical Features

- **Event-driven Design** – No cyclic polling; all operations are triggered by events.
- **Generic Output Selection** – The `Output` data input allows mapping to any logiBUS DQ channel (Q1…Q8) without modification.
- **Rising-Edge Echo** – `EO1` only fires when the latched output turns on, providing a clean positive-transition notification.
- **Integrated Output Stage** – Combines latch, physical output, and edge detection in a single reusable unit.
- **Non-Blocking** – Suitable for distributed control applications following the IEC 61499 execution model.

## State Overview

The internal latch has two stable states:

| State            | Output `Q` | `EO1` Emission         |
|------------------|------------|------------------------|
| Reset (default)  | `FALSE`    | No                     |
| Set              | `TRUE`     | Emitted on rise to `TRUE` |

The `EO1` event is only produced during the `FALSE` → `TRUE` transition; RESET operations do not trigger `EO1`.

## Application Scenarios

- **Discrete Output Control** – Switching a digital actuator (e.g., valve, lamp, motor starter) on and off via events.
- **Acknowledgment Signaling** – Providing a confirmation event to a supervisory controller when the output has been successfully switched on.
- **Reusable Library Component** – The block can be instantiated multiple times in a system, each bound to a different output channel.
- **Training / Educational Use** – Suitable for IEC 61499 and 4diac demonstrations of latch, edge detection, and output mapping concepts.

## Comparison with Similar Blocks

| Block | Key Difference |
|-------|----------------|
| **E_SR (standalone)** | Only provides latch functionality without integrated output or echo. |
| **E_RS** | Uses a reset-dominant latch behavior; no rising-edge echo or output stage. |
| **logiBUS_QX (direct)** | Writes an output directly but lacks latch memory and event echo. |
| **SR_TO_QX_TRIG_EO** | Combines latch, physical output update, and rising-edge echo in one block; self-contained and reusable. |

## Conclusion

The **SR_TO_QX_TRIG_EO** subapplication is a compact, reusable building block for event-based digital output control with latch semantics and a rising-edge acknowledgment event. Its generic output selection makes it flexible for a wide range of logiBUS-based applications, while its clean event interface integrates seamlessly into IEC 61499 systems. By bundling latch, output driver, and edge detection, it reduces wiring effort and improves maintainability.
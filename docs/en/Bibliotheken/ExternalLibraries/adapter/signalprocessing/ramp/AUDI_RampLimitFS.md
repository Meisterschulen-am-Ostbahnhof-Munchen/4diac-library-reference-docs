# AUDI_RampLimitFS

![AUDI_RampLimitFS](./AUDI_RampLimitFS.svg)

* * * * * * * * * *
## Introduction

The **AUDI_RampLimitFS** function block is a composite wrapper designed to integrate the ramp functionality of the `RampLimitFS` FB with an AUDI adapter interface. It provides a seamless connection between unidirectional AUDI sockets/plugs and the internal ramp generator, while also exposing latched status flags (`qAtZero` and `qAtFull`) through AX adapters. The FB accepts a setpoint via a `LOAD` socket, applies ramp-up/down or jump commands, and outputs the ramped value via the `OUT` plug. The two status plugs indicate whether the current output has reached the defined minimum or maximum value, with the latched behavior ensuring the flags persist until the next corresponding event.

## Interface Structure

### **Event Inputs**

| Event     | Type    | Comment                               |
|-----------|---------|---------------------------------------|
| `INIT`    | EInit   | Initialization request, sets internal parameters and initializes the ramp generator. |
| `ZERO`    | Event   | Jump to `VAL_ZERO` (minimum value).   |
| `DOWN_FAST` | Event  | Ramp down at fast speed.              |
| `DOWN_SLOW` | Event  | Ramp down at slow speed.              |
| `UP_SLOW` | Event   | Ramp up at slow speed.                |
| `UP_FAST` | Event   | Ramp up at fast speed.                |
| `FULL`    | Event   | Jump to `VAL_FULL` (maximum value).   |

### **Event Outputs**

| Event   | Type    | Comment                          |
|---------|---------|----------------------------------|
| `INITO` | EInit   | Initialization confirm signal.   |

### **Data Inputs**

| Name       | Type  | Initial Value | Comment                               |
|------------|-------|---------------|---------------------------------------|
| `VAL_ZERO` | DINT  | 0             | Minimum value of the ramp range.      |
| `SLOW`     | DINT  | 1             | Step size for slow ramp operations.   |
| `FAST`     | DINT  | 10            | Step size for fast ramp operations.   |
| `VAL_FULL` | DINT  | 100           | Maximum value of the ramp range.      |

### **Data Outputs**

There are no direct data outputs; all data is communicated through adapters.

### **Adapters**

| Name      | Type                                   | Direction | Comment                                                                 |
|-----------|----------------------------------------|-----------|-------------------------------------------------------------------------|
| `LOAD`    | `adapter::types::unidirectional::AUDI` | Socket    | Receives a load request (`E1`) and a process value (`D1`, type UDINT) to be used as the ramp setpoint. |
| `OUT`     | `adapter::types::unidirectional::AUDI` | Plug      | Delivers the ramped output value (`D1`, type UDINT) and an output event (`E1`). |
| `qAtZero` | `adapter::types::unidirectional::AX`   | Plug      | Status flag: `TRUE` when the output equals `VAL_ZERO`; latched via an internal E_D_FF. |
| `qAtFull` | `adapter::types::unidirectional::AX`   | Plug      | Status flag: `TRUE` when the output equals `VAL_FULL`; latched via an internal E_D_FF. |

## Functionality

The `AUDI_RampLimitFS` block encapsulates a `RampLimitFS` FB and provides a convenient adapter-based interface. Internally, the following data flow is implemented:

1. **Load Request**: An event on the `LOAD` socket (`E1`) triggers a conversion of the incoming UDINT value (`D1`) to DINT using `F_UDINT_TO_DINT`. The converted value is passed as the setpoint (`PV`) to the `RampLimitFS` FB.

2. **Ramp Control**: The events `ZERO`, `DOWN_FAST`, `DOWN_SLOW`, `UP_SLOW`, `UP_FAST`, and `FULL` are directly forwarded to the corresponding event inputs of the `RampLimitFS` FB. These events control how the output moves towards the setpoint:
   - `ZERO` causes an immediate jump to `VAL_ZERO`.
   - `FULL` causes an immediate jump to `VAL_FULL`.
   - The ramp events (`DOWN_*`, `UP_*`) cause the output to move stepwise in the specified direction and speed.

3. **Output Generation**: When the `RampLimitFS` completes a step or a jump (indicated by its `CNF` event), the current DINT output is converted back to UDINT via `F_DINT_TO_UDINT` and delivered on the `OUT` plug along with an `E1` event.

4. **Status Latching**: The `RampLimitFS` provides boolean flags `qAtZero` and `qAtFull` indicating whether the current output is exactly at `VAL_ZERO` or `VAL_FULL`. These flags are latched using two E_D_FF (D-type flip-flop) FBs, one per status. On each `CNF` event, the current flag value is sampled and stored; the latched value is then propagated to the corresponding AX plug (`qAtZero.D1` or `qAtFull.D1`) along with an event `E1`.

5. **Initialization**: The `INIT` event transfers the parameters (`VAL_ZERO`, `SLOW`, `FAST`, `VAL_FULL`) to the internal `RampLimitFS` and initiates its own initialization. Upon completion, `INITO` is emitted.

## Technical Features

- **Adapter Integration**: Combines AUDI and AX adapter types for a clean, IEC 61499‑compliant interface.
- **Type Conversion**: Automatically converts between UDINT (adapter data) and DINT (internal ramp calculations) using standard conversion FBs.
- **Latched Status**: The `qAtZero` and `qAtFull` flags are latched via edge‑triggered D flip‑flops, ensuring that the status remains stable until the next output change.
- **Configurable Ramp Limits**: The minimum (`VAL_ZERO`) and maximum (`VAL_FULL`) values are user‑definable, as are the step sizes for slow and fast ramping.
- **Composite Architecture**: Built entirely from standard FBs (`RampLimitFS`, conversion FBs, and E_D_FF), making it easy to inspect and modify.

## State Overview

The behavior of the internal `RampLimitFS` can be described in terms of operational states, which are reflected in the events and the status flags:

- **Idle / Initialized**: After `INIT`, the output is typically set to `VAL_ZERO` (or the last value). No ramp operations are active.
- **Jump to Zero / Full**: When `ZERO` or `FULL` is issued, the output directly moves to the respective limit. The corresponding status flag (`qAtZero` or `qAtFull`) is set and latched.
- **Ramping**: During `UP_*` or `DOWN_*` operations, the output changes stepwise by `SLOW` or `FAST` increments/decrements per cycle. The process continues until the setpoint is reached or a different command is issued.
- **Limit Reached**: When the output reaches either limit, the associated status flag becomes active and remains latched until a new command moves the output away.

## Application Scenarios

- **Motion Control**: Adjusting a motor speed or position with configurable acceleration/deceleration rates, using fast/slow ramps to avoid mechanical stress.
- **Process Automation**: Smoothly transitioning a process variable (e.g., temperature, flow) between setpoints, with automatic clamping to safety minimum/maximum values.
- **HMI Integration**: The AUDI adapter format is ideal for connecting to visualization or supervisory systems that exchange UDINT data, while the AX status plugs provide simple boolean indication for limit states.

## Comparison with Similar Blocks

- **vs. Standard `RampLimitFS`**: The `AUDI_RampLimitFS` adds an adapter layer and latched status outputs. It simplifies integration by hiding the conversion steps and provides a more standardized interface for industrial applications.
- **vs. Simple Ramp FBs without status**: Many basic ramp blocks only output the ramped value; this FB additionally provides explicit, latched limit indicators, which can be used for alarm or interlock logic.
- **vs. PID or other closed‑loop controllers**: Unlike controllers, this block does not perform feedback regulation; it only generates a time‑based ramp signal towards a given setpoint.

## Conclusion

**AUDI_RampLimitFS** is a robust, adaptable function block that encapsulates a proven ramp generator with a modern adapter-based interface. Its use of AUDI and AX adapters promotes interoperability, while the internal type conversions and latched status flags reduce the effort required for integration and monitoring. The block is suitable for a wide range of applications where controlled, limit‑bounded transitions of a numeric output are needed.
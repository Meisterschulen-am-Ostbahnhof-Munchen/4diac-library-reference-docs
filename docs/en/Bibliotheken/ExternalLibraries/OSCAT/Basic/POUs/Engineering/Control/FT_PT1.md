# FT_PT1

![FT_PT1](FT_PT1.svg)

* * * * * * * * * *

## Introduction

`FT_PT1` is a first-order low-pass filter (PT1 element) from the OSCAT library. It is used to smooth analog signals and reduce noise with a programmable time constant `TM` and gain factor `K`.

The block is implemented as a `SimpleFB` (stateful function block with `EINIT`/`REQ`/`RST`) and performs discrete-time integration according to the PT1 transfer function:

$$T_M \cdot \frac{dy}{dt} + y(t) = K \cdot x(t)$$

## Interface Structure

### **Event Inputs**

| Name | Type | Description | With Data |
| :--- | :--- | :----------------------- | :-------- |
| `EINIT` | `Event` | Service initialization of the filter | |
| `REQ` | `Event` | Execution request for a new calculation cycle | `in`, `TM`, `K` |
| `RST` | `Event` | Resets the filter output `out` to 0.0 | |

### **Event Outputs**

| Name | Type | Description | With Data |
| :--- | :--- | :----------------------- | :-------- |
| `INITO` | `Event` | Initialization confirmation | |
| `CNF` | `Event` | Execution confirmation after calculation | `out` |

### **Input Vars**

| Name | Type | Initial Value | Description |
| :--- | :--- | :------------ | :------------------- |
| `in` | `REAL` | `0.0` | Analog input signal |
| `TM` | `TIME` | `T#0s` | Filter time constant |
| `K` | `REAL` | `1.0` | Gain factor (proportional coefficient) |

### **Output Vars**

| Name | Type | Description |
| :--- | :--- | :----------------------- |
| `out` | `REAL` | Filtered output value |

## How It Works

1. **Initialization (`EINIT`)**:  
   Prepares the block, resets internal timestamp tracking, and signals readiness via `INITO`.

2. **Cyclic Calculation (`REQ`)**:  
   On each `REQ` event, the block calculates the elapsed time since the last call ($\Delta t$) using system time. The time constant `TM` is converted to seconds via `TIME_TO_REAL`.  
   The new output value is calculated according to:
   
   $$\text{out}_{\text{new}} = \text{out}_{\text{old}} + \left( K \cdot \text{in} - \text{out}_{\text{old}} \right) \cdot \frac{\Delta t}{TM}$$
   
   and provided via `CNF`.

3. **Filter Reset (`RST`)**:  
   Resets the stored internal output value `out` immediately to `0.0`.

## Technical Features

- **Accurate Timebase**: The time difference is measured with microsecond accuracy, ensuring precise filter behavior independent of cycle time variations.
- **RST Handling**: A reset event ensures the filter output can be instantly cleared to zero when required (e.g., sensor shutdown).
- **Project-Specific Time Conversion**: Uses the project-specific helper function `TIME_TO_REAL.fct` for clean conversion of the `TIME` input `TM` into seconds (`REAL`).

## Application Scenarios

- Smoothing highly fluctuating sensor readings (e.g., pressure, temperature, or voltage signals).
- Noise reduction for control loop inputs.
- Soft start/ramp adjustment for setpoints.

## See Also

- [`FT_PT1_AR`](FT_PT1_AR.md) – AR adapter wrapper for `FT_PT1`.
- [`FT_PT2`](FT_PT2.md) – Second-order low-pass filter.
- [`TIME_TO_REAL`](TIME_TO_REAL.md) – Time conversion helper function.

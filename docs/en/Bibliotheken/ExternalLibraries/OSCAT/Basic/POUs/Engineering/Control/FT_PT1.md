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
| `RST` | `Event` | Resets the filter output and timebase (`out = K * in`) | |

### **Event Outputs**

| Name | Type | Description | With Data |
| :--- | :--- | :----------------------- | :-------- |
| `INITO` | `Event` | Initialization confirmation | |
| `CNF` | `Event` | Execution confirmation after calculation | `delta_t`, `out` |

### **Input Vars**

| Name | Type | Initial Value | Description |
| :--- | :--- | :------------ | :------------------- |
| `in` | `REAL` | `0.0` | Analog input signal |
| `TM` | `TIME` | `T#0s` | Filter time constant. If `TM = T#0s`, filtering is bypassed and `out = K * in`. |
| `K` | `REAL` | `1.0` | Gain factor (proportional coefficient) |

### **Output Vars**

| Name | Type | Description |
| :--- | :--- | :----------------------- |
| `delta_t` | `UDINT` | Elapsed time since last call in microseconds ($\mu s$) |
| `out` | `REAL` | Filtered output value |

## How It Works

1. **Initialization (`EINIT`)**:  
   Resets initialization state, sets `out = 0.0`, and signals readiness via `INITO`.

2. **Cyclic Calculation (`REQ`)**:  
   On each `REQ` event, the block calculates elapsed time since the last call ($\Delta t$) in microseconds using `T_PLC_US()`.
   - If the block is not yet initialized or `TM = T#0s`, `RST` is invoked internally and `out = K * in` is output directly.
   - For `TM > T#0s`, the new output value is calculated according to:
   
     $$\text{out}_{\text{new}} = \text{out}_{\text{old}} + \left( K \cdot \text{in} - \text{out}_{\text{old}} \right) \cdot \frac{\Delta t}{TM}$$
   
   - To prevent denormalized float underruns, values $|out| < 1.0 \times 10^{-20}$ are automatically zeroed.

3. **Filter Reset (`RST`)**:  
   Sets `out` immediately to scaled input `K * in` and updates the internal timestamp `last := T_PLC_US()`. This prevents spurious step jumps caused by elapsed time when resuming calculations.

## Technical Features

- **Cycle-Independent Timebase**: Time differences are measured in microseconds via `T_PLC_US()`, compensating for call cycle variations.
- **Filter Bypass at `TM = 0`**: When `TM = T#0s`, damping is disabled and the input signal is passed through scaled by `K`.
- **Clean Reset Recovery**: `RST` updates `last` timestamp so no outlier steps occur when filter operation resumes.
- **Project-Specific Time Conversion**: Uses the project-specific helper function `TIME_TO_REAL.fct` for clean conversion of `TIME` input `TM` to seconds (`REAL`).

## Application Scenarios

- Smoothing fluctuating sensor readings (e.g., pressure, temperature, speed).
- Noise reduction for control loop inputs.
- Soft start/ramp adjustment for setpoints.

## See Also

- [`FT_PT1_AR`](FT_PT1_AR.md) – AR adapter wrapper for `FT_PT1`.
- [`FT_PT2`](FT_PT2.md) – Second-order low-pass filter.
- [`TIME_TO_REAL`](TIME_TO_REAL.md) – Time conversion helper function.

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
| `RST` | `Event` | Invalidates initialization state (`init = FALSE`) for re-seeding on next `REQ` | |

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
   - If the block is not yet initialized (`init = FALSE`) or `TM = T#0s`, the filter state is (re)initialized: `init := TRUE`, `out := K * in` is seeded directly using fresh input data, `delta_t := 0`, and the timestamp baseline `last` is refreshed.
   - For `TM > T#0s`, the new output value is calculated according to:
   
     $$\text{out}_{\text{new}} = \text{out}_{\text{old}} + \left( K \cdot \text{in} - \text{out}_{\text{old}} \right) \cdot \frac{\Delta t}{T_{\text{eff}}}$$
   
     where $\Delta t = \text{delta\_t} \cdot 10^{-6}\,\text{s}$ is the measured call interval in seconds (from `T_PLC_US()`), $T_M = \text{TIME\_TO\_REAL}(TM)$ is the filter time constant in seconds, and $T_{\text{eff}} = \max(T_M, \Delta t)$ is the effective time constant in seconds. When the call interval $\Delta t$ exceeds the configured filter time $T_M$, $T_{\text{eff}}$ is clamped to $\Delta t$, capping the weighting factor $\frac{\Delta t}{T_{\text{eff}}}$ to at most $1.0$.
   - To prevent denormalized float underruns, values $|out| < 1.0 \times 10^{-20}$ are automatically zeroed.

3. **Filter Reset (`RST`)**:  
   Invalidates the initialization state (sets `init := FALSE`) without modifying `out` or timestamps immediately. This defers seeding until the next `REQ` event, ensuring fresh input data is sampled (rather than stale or default `0.0` values) and refreshing the timebase baseline.

## Technical Features

- **Cycle-Independent Timebase**: Time differences are measured in microseconds via `T_PLC_US()`, compensating for call cycle variations.
- **Clamped Effective Filter Time Constant ($T_{\text{eff}} = \max(T_M, \Delta t)$)**: If call interval $\Delta t$ exceeds filter time $T_M$, the block caps the discretization factor $\frac{\Delta t}{T_{\text{eff}}}$ to at most $1.0$. This prevents Euler instability and overshoot when call cycles occur slower than $T_M$.
- **Filter Bypass at `TM = 0`**: When `TM = T#0s`, damping is disabled and the input signal is passed through scaled by `K`.
- **Clean Reset Recovery**: `RST` invalidates the initialization state (`init := FALSE`) without immediate output overwrite. The subsequent `REQ` event samples fresh input data, seeds `out := K * in`, and resets the timing baseline `last`, preventing spurious step jumps from stale input values.
- **Project-Specific Time Conversion**: Uses the project-specific helper function `TIME_TO_REAL.fct` for clean conversion of `TIME` input `TM` to seconds (`REAL`).

## Application Scenarios

- Smoothing fluctuating sensor readings (e.g., pressure, temperature, speed).
- Noise reduction for control loop inputs.
- Soft start/ramp adjustment for setpoints.

## See Also

- [`FT_PT1_AR`](FT_PT1_AR.md) – AR adapter wrapper for `FT_PT1`.
- [`FT_PT2`](FT_PT2.md) – Second-order low-pass filter.
- [`TIME_TO_REAL`](TIME_TO_REAL.md) – Time conversion helper function.

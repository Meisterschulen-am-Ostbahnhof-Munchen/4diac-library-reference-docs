# FT_PT2_AR

## Introduction

`FT_PT2_AR` is an AR adapter wrapper around the OSCAT 2nd-order low-pass filter block `FT_PT2`. It enables 2nd-order low-pass filtering with configurable time constant `TM`, damping `D`, and gain `K` within purely adapter-based IEC 61499 applications.

The time constant `TM` is provided via an `ATM` adapter socket (per section 13 of the `iec61499-creator` skill), while damping `D` and gain `K` remain plain `InputVars`. An internal `E_D_FF_ANY` D flip-flop ensures that output event `AR_OUT.E1` is fired unconditionally on the first cycle and subsequently only when an actual value change occurs.

## Interface Structure

### **Event Inputs**

| Name | Type | Description |
| :--- | :--- | :----------- |
| `INIT` | `EInit` | Service initialization, passed to `FT_PT2.INIT` |
| `RST` | `Event` | Resets the filter output via `FT_PT2.RST` |

### **Event Outputs**

| Name | Type | Description |
| :--- | :--- | :----------- |
| `INITO` | `EInit` | Initialization confirmation from `FT_PT2` |

### **Input Vars**

| Name | Type | Initial Value | Description |
| :--- | :--- | :------------ | :----------- |
| `D` | `REAL` | `0.0` | Filter damping factor (e.g., 0.707 for Butterworth response) |
| `K` | `REAL` | `1.0` | Gain factor |

### **Output Vars**

None. Output is provided exclusively through the adapter plug `AR_OUT`.

### **Adapters**

| Interface | Direction | Adapter Type | Data Type | Description |
| :--- | :--- | :--- | :--- | :--- |
| `AR_IN` | Socket | `AR` | `REAL` | Input signal to be filtered |
| `TM` | Socket | `ATM` | `TIME` | Filter time constant (`TM.D1` passed to `FT_PT2.TM`) |
| `AR_OUT` | Plug | `AR` | `REAL` | Filtered output value (`FT_PT2.out` via `E_D_FF_ANY`) |

## How It Works

The block embeds `FT_PT2` inside an internal FB network:

1. **Signal Processing**:  
   An event on `AR_IN.E1` triggers `FT_PT2.REQ`. `AR_IN.D1` is supplied to `FT_PT2.in`.
2. **Parameterization**:  
   `TM.D1` is read from the `ATM` socket. `D` and `K` are supplied as parameters.
3. **2nd-Order Stateful Integration**:  
   `FT_PT2` filters the signal internally through two cascaded integration stages (`INTEGRATE`).
4. **Change Filtering (`E_D_FF_ANY`)**:  
   `FT_PT2.CNF` drives internal `E_D_FF_ANY`. The flip-flop forwards the computed output value to `AR_OUT` unconditionally on the first cycle. On subsequent cycles, `AR_OUT.E1` and `AR_OUT.D1` are updated only when the computed filter value differs from the previous value.
5. **Reset & Initialization**:  
   `INIT` controls `FT_PT2.INIT`. `RST` clears the internal memory of both integrator stages.

## Technical Features

- **2nd-Order Response**: Provides steeper attenuation of high-frequency noise (12 dB/octave roll-off) compared to a PT1 filter.
- **Integrated Event Decoupling**: Eliminates redundant adapter events in downstream SubApps.
- **`ATM` Socket for Timing**: Adheres to project design rules for timing constants.

## Application Scenarios

- High-frequency noise suppression on analog signals (e.g., force, pressure, or ground speed).
- Vibration damping in closed-loop control systems.

## See Also

- [`FT_PT2`](FT_PT2.md) – Underlying OSCAT block.
- [`FT_PT1_AR`](FT_PT1_AR.md) – AR adapter wrapper for 1st-order PT1 filter.
- [`FT_DERIV_AR`](FT_DERIV_AR.md) – AR adapter wrapper for differentiator.

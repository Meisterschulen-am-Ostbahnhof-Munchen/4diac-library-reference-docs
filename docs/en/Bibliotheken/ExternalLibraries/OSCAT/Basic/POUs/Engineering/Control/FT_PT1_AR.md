# FT_PT1_AR

## Introduction

`FT_PT1_AR` is an AR adapter wrapper around the OSCAT low-pass filter block `FT_PT1`. It encapsulates first-order low-pass filtering behind a purely adapter-based interface for IEC 61499 applications.

The time constant `TM` is provided as an `ATM` socket (per section 13 of the `iec61499-creator` skill), while gain factor `K` remains a plain `InputVar`. Internally, the block uses an `E_D_FF_ANY` D flip-flop to detect changes in the filtered value and fire adapter events only upon initial evaluation or when an actual change occurs.

## Interface Structure

### **Event Inputs**

| Name | Type | Description |
| :--- | :--- | :----------- |
| `INIT` | `EInit` | Service initialization, passed to `FT_PT1.INIT` |
| `RST` | `Event` | Invalidates filter state via `FT_PT1.RST` (`init := FALSE`) |

### **Event Outputs**

| Name | Type | Description |
| :--- | :--- | :----------- |
| `INITO` | `EInit` | Initialization confirmation from `FT_PT1` |

### **Input Vars**

| Name | Type | Initial Value | Description |
| :--- | :--- | :------------ | :----------- |
| `K` | `REAL` | `1.0` | Gain factor, passed to `FT_PT1.K` |

### **Output Vars**

None. Output is provided exclusively through the adapter plug `AR_OUT`.

### **Adapters**

| Interface | Direction | Adapter Type | Data Type | Description |
| :--- | :--- | :--- | :--- | :--- |
| `AR_IN` | Socket | `AR` | `REAL` | Input signal to be filtered |
| `TM` | Socket | `ATM` | `TIME` | Filter time constant (`TM.D1` passed to `FT_PT1.TM`) |
| `AR_OUT` | Plug | `AR` | `REAL` | Filtered output value (`FT_PT1.out` via `E_D_FF_ANY`) |

## How It Works

The block connects `FT_PT1` (OSCAT) and `E_D_FF_ANY` in an internal network:

1. **Signal Processing**:  
   An event on `AR_IN.E1` triggers `FT_PT1.REQ`. `AR_IN.D1` supplies the raw analog value.
2. **Time Constant & Re-initialization**:  
   The filter time `TM.D1` is supplied by the `ATM` socket and passed to `FT_PT1.TM`. An event on `TM.E1` also directly triggers `FT_PT1.RST` internally, invalidating the filter state (`init := FALSE`) so that the subsequent `AR_IN.E1` request reinitializes the filter (`out := K * in`) using fresh input data and the updated time constant.
3. **Change Detection & Decoupling (`E_D_FF_ANY`)**:  
   After calculation, `FT_PT1.CNF` triggers `CLK` on internal `E_D_FF_ANY`. The flip-flop emits the initial output value on the first clock after start. On subsequent cycles, `AR_OUT.E1` and `AR_OUT.D1` are updated **only** when a value change occurs.
4. **Reset & Initialization**:  
   - `INIT` controls `FT_PT1.INIT` and confirms via `INITO`. Unconnected `INIT` events auto-fire once upon deployment.
   - `RST` as well as any incoming `TM.E1` event are passed to `FT_PT1.RST` to invalidate the filter state (`init := FALSE`). The next `AR_IN.E1` request performs clean re-initialization.

## Technical Features

- **Clean Adapter Boundary**: Prevents direct accessing of internal `.E1`/`.D1` structures across SubApp networks.
- **Automatic Filter Re-initialization on TM Change**: Receiving a `TM.E1` event internally triggers `FT_PT1.RST` (setting `init := FALSE`). The subsequent `AR_IN.E1` request reinitializes `out := K * in` with fresh input data and resets the timing baseline, so modified time constants take effect cleanly without jump artifacts or stale seeding.
- **Event Traffic Reduction**: `E_D_FF_ANY` prevents unnecessary event flooding down the processing chain for unchanged values.
- **`ATM` Socket Convention**: Timing parameters are routed via `ATM` sockets rather than bare variables (fed at instantiation site e.g., via `initval_ATM`).

## Application Scenarios

- Smoothing analog sensor readings (e.g., pressure, temperature, speed) in adapter-based SubApp architectures.
- Noise filtering prior to thresholding or hysteresis blocks (`AR_D_FF_HYS_TMIN`).

## See Also

- [`FT_PT1`](FT_PT1.md) – Underlying OSCAT block.
- [`FT_PT2_AR`](FT_PT2_AR.md) – AR adapter wrapper for 2nd-order PT2 filter.
- [`FT_DERIV_AR`](FT_DERIV_AR.md) – AR adapter wrapper for differentiator.

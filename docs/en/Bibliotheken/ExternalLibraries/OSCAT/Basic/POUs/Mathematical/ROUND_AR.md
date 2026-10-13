# ROUND_AR

![ROUND_AR_network](./ROUND_AR_network.svg)

* * * * * * * * * *

## Introduction

`ROUND_AR` is an AR adapter wrapper around the OSCAT function `OSCAT::Basic::POUs::Mathematical::ROUND` (rounds `in` to `N` decimal places, 0 to 8 digits). It bridges between adapters and OSCAT without either base library depending on the other. `N` is passed through as a plain input variable (default 2).

![ROUND_AR](ROUND_AR.svg)

## Interface Structure

### **Event Inputs**

- **REQ**: explicit execution request (passed on to `ROUND.REQ`, in addition to `AR_IN.E1`)
    - **Associated data**: `N`

### **Event Outputs**

The function block has no direct event outputs.

### **Data Inputs**

- **N** (INT): number of decimal places (passed on to `ROUND.N`); placeholder default 2, overridable via parameter

### **Data Outputs**

The function block has no direct data outputs.

### **Adapters**

- **AR_IN** (socket, `adapter::types::unidirectional::AR`): input signal (REAL)
- **AR_OUT** (plug, `adapter::types::unidirectional::AR`): value rounded to `N` decimal places (REAL)

## Functionality

- `AR_IN.E1` → `ROUND.REQ` and `AR_IN.D1` → `ROUND.in`; `N` → `ROUND.N`.
- `REQ` also goes to `ROUND.REQ`, in parallel to `AR_IN.E1`.
- `ROUND.CNF` → `E_D_FF_ANY_1.CLK`; the function result goes to `E_D_FF_ANY_1.D`.
- `E_D_FF_ANY_1` ([`iec61499::events::E_D_FF_ANY`](../../../../../StandardLibraries/events/E_D_FF_ANY.md)) filters changes: `E_D_FF_ANY_1.EO` → `AR_OUT.E1` and `E_D_FF_ANY_1.Q` → `AR_OUT.D1`, but only if the rounded result has actually changed since the last call.

## Technical Details

- The explicit `REQ` with `With N` was added because `N` would otherwise only be read via a data connection; per IEC 61499, variables not bound via `With` are not read reliably. `REQ` allows manual or periodic execution in addition to automatic triggering via `AR_IN.E1`
- Change filter: `AR_OUT.E1` fires only when the rounded value actually changes (same pattern as `CTRL_PI_AR`/`FT_DERIV_AR`, there with `E_D_FF_ANY_3`/`_4`)
- `ROUND` is a pure OSCAT function without an adapter interface; the bridge lives in the third library project `OSCAT_adapter`

## Application Scenarios

- Rounding displayed or forwarded measured values to a fixed number of decimal places in adapter networks

## ⚖️ Comparison with Similar Blocks

`ROUND_STEP_AR` rounds to an arbitrary grid (e.g. 0.25) instead of only to decimal places and uses `ROUND_AR` internally (`ROUND(AR_IN / RASTER) * RASTER`). [`FLOOR2_AR`](FLOOR2_AR.md), in contrast, returns a rounded-down integer as a DINT.

## Conclusion

`ROUND_AR` provides `ROUND` as an adapter block from `AR` to `AR`, rounds to `N` decimal places and outputs the result only on change.

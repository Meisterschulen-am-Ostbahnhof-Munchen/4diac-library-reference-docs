# CEIL2_AR

![CEIL2_AR_network](./CEIL2_AR_network.svg)

* * * * * * * * * *

## Introduction

`CEIL2_AR` is an adapter wrapper around `OSCAT::Basic::POUs::Mathematical::CEIL2` (DINT variant of CEIL for values outside the INT range). It connects the `AR` adapter with the OSCAT function `CEIL2` and outputs the result as an `ADI` adapter (smallest integer greater than or equal to X (DINT)). The wrapper bridges the adapter and OSCAT libraries without either base library depending on the other.

![CEIL2_AR](CEIL2_AR.svg)

## Interface Structure

### **Event Inputs**

No direct event inputs - events arrive via the adapter `AR_IN`.

### **Event Outputs**

No direct event outputs - events leave via the adapter `ADI_OUT`.

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

- **AR_IN** (socket, `adapter::types::unidirectional::AR`): input signal (`REAL`)
- **ADI_OUT** (plug, `adapter::types::unidirectional::ADI`): smallest integer greater than or equal to X (DINT)

## Function Blocks (FBs) Used

- **`CEIL2`** (`OSCAT::Basic::POUs::Mathematical::CEIL2`): rounding function, see [CEIL2](./CEIL2.md)
- **`E_D_FF_ANY_1`** (`iec61499::events::E_D_FF_ANY`): change filter

## Functionality

- `AR_IN.E1` → `CEIL2.REQ`; `AR_IN.D1` → `CEIL2.X`
- `CEIL2.CNF` → `E_D_FF_ANY_1.CLK`; return value of `CEIL2` → `E_D_FF_ANY_1.D`
- `E_D_FF_ANY_1.EO` → `ADI_OUT.E1`; `E_D_FF_ANY_1.Q` → `ADI_OUT.D1`

## Technical Details

- `E_D_FF_ANY_1` (standard library `events-3.0.0`) filters changes: `ADI_OUT.E1` only fires when the result actually changed since the last call. Same pattern as the `FT_DERIV_AR`/`CTRL_PI_AR` wrappers.
- **Why a separate wrapper?** `CEIL2` is a pure OSCAT `Function` without an adapter interface. `adapter` should not depend on `OSCAT` (and vice versa); the bridge therefore lives in the third library project `OSCAT_adapter`, which depends on both.

## Application Scenarios

- Rounding an `AR` signal in an adapter chain to an integer (`DINT`)

## Summary

`CEIL2_AR` rounds an `AR` value with `CEIL2` and outputs the result as an `ADI` adapter only when it changes.

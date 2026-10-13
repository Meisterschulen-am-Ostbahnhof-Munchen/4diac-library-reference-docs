# E_CALIBRATE_2P

![E_CALIBRATE_2P_ecc](./E_CALIBRATE_2P_ecc.svg)

![E_CALIBRATE_2P](E_CALIBRATE_2P.svg)

* * * * * * * * * *

## Introduction

`E_CALIBRATE_2P` is an event-driven two-point calibration (low/high) with independently stored points. It replaces [E_CALIBRATE](E_CALIBRATE.md) (`E_CALIBRATE_SQ` behaviour): the two points are stored completely independently of each other and only combined into a line on reading (`REQ`). There is therefore no enforced order of `EI_LOW`/`EI_HIGH`.

## Interface Structure

### **Event Inputs**

- **REQ**: normal execution request, calculates `Y`. Provides `X`, `LOW_REF`, `HIGH_REF`, `X_LOW`, `X_HIGH`.
- **EI_LOW**: calibrates the low point (`X_LOW := X`). Provides `X`.
- **EI_HIGH**: calibrates the high point (`X_HIGH := X`). Provides `X`.

### **Event Outputs**

- **CNF**: confirms `REQ`, provides `Y`.
- **EO_LOW**: low point calibrated, provides `X_LOW`.
- **EO_HIGH**: high point calibrated, provides `X_HIGH`.

### **Data Inputs**

- **X** (REAL): raw input value.
- **LOW_REF** (REAL): target output `Y` at the low calibration point.
- **HIGH_REF** (REAL): target output `Y` at the high calibration point.

### **Data Outputs**

- **Y** (REAL): calibrated output value.

### **In/Out Variables (InOut)**

- **X_LOW** (REAL, initial value `0.0`): stored raw value at the low calibration point.
- **X_HIGH** (REAL, initial value `1.0`): stored raw value at the high calibration point.

### **Adapters**

No adapters available.

## Functionality

Formula: `Y = LOW_REF + (X - X_LOW) * (HIGH_REF - LOW_REF) / (X_HIGH - X_LOW)`

The block has a start state `START` and three action states directly reachable from it:

- **REQ**: calculates `Y` live by linear interpolation between the two stored points (without clipping, as with `E_CALIBRATE_SQ`) and fires `CNF`. If `X_HIGH - X_LOW = 0`, then `Y := LOW_REF`.
- **C_LOW**: stores the current raw value in `X_LOW` and fires `EO_LOW`.
- **C_HIGH**: stores the current raw value in `X_HIGH` and fires `EO_HIGH`.

Every action state returns immediately to `START`.

**Calibration (free order):**

1. **Low (`EI_LOW`)**: apply the low reference, set `LOW_REF` to the desired output, fire `EI_LOW` → `X_LOW := X`
2. **High (`EI_HIGH`)**: apply the high reference, set `HIGH_REF` to the desired output, fire `EI_HIGH` → `X_HIGH := X`

Either point can be recalibrated on its own at any later time without repeating the other.

## Technical Details

- **Independent points instead of incremental OFFSET/SCALE calculation:** `E_CALIBRATE_SQ` computes OFFSET/SCALE step by step (EICO sets OFFSET from a live SCALE, EICS computes both from internally remembered values) and is only mathematically valid after at least one EICO. That required an ECC-enforced order to avoid silently wrong offsets. `E_CALIBRATE_2P` stores `X_LOW`/`X_HIGH` like `E_CALIBRATE_3P` stores its points, with no data dependency between `EI_LOW` and `EI_HIGH`.
- No clipping of `Y` to `LOW_REF..HIGH_REF` (unlike `E_CALIBRATE_3P`).
- Degenerate calibration points (`X_HIGH = X_LOW`) yield `LOW_REF` instead of a division by zero.

## State Overview

```text
START --REQ-----> REQ    --1--> START   (normal operation)
START --EI_LOW--> C_LOW  --1--> START   (calibrate low point)
START --EI_HIGH-> C_HIGH --1--> START   (calibrate high point)
```

## Application Scenarios

- Two-point calibration of analog inputs (e.g. sensors) when the calibration trigger is already available as an event
- Readjusting only one point

## ⚖️ Comparison with Similar Building Blocks

- [E_CALIBRATE](E_CALIBRATE.md): variant with `EICO`/`EICS` (`OFFSET`/`SCALE`) in which `EICS` only yields a consistent result after `EICO`
- [E_CALIBRATE_3P](E_CALIBRATE_3P.md): three-point variant with piecewise interpolation and clipping

## Conclusion

`E_CALIBRATE_2P` provides an event-driven two-point calibration with independently triggerable calibration points and no enforced order.

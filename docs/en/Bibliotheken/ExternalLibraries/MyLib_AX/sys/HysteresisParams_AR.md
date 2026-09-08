# HysteresisParams_AR

![HysteresisParams_AR_network](./HysteresisParams_AR_network.svg)

* * * * * * * * * *

## Introduction

`HysteresisParams_AR` bundles the three parameters `MI` (mean/target value), `DEAD` (dead zone), and `HYSTERESIS` (additional hysteresis) as fixed `AR` adapter constants for `DualHysteresis_AR_AX`/`DualHysteresis_AR_A2X`. Instead of repeating three `initval_AR` instances individually in each exercise/subapp, this building block combines them.


## Function Blocks (FBs) Used

### Sub-Blocks: HysteresisParams_AR

- **Type**: SubAppType
- **Internal FBs Used**:

- **initval_AR_MI** / **initval_AR_DEAD** / **initval_AR_HYSTERESIS**: each `adapter::types::unidirectional::AR::initval::initval_AR` — converts a `REAL` input value into a fixed `AR` adapter plug.

- **How it Works**: The three `InputVars`, `rMI`, `rDEAD`, and `rHYSTERESIS` are each converted into the corresponding `AR` plugs (`MI`, `DEAD`, and `HYSTERESIS`) via a `initval_AR` instance.

## Program Flow and Connections

1. `rMI` → `initval_AR_MI.INIT_VAL`; `rDEAD` → `initval_AR_DEAD.INIT_VAL`; `rHYSTERESIS` → `initval_AR_HYSTERESIS.INIT_VAL` (Data connections).

2. `initval_AR_MI.OUT` → `MI`; `initval_AR_DEAD.OUT` → `DEAD`; `initval_AR_HYSTERESIS.OUT` → `HYSTERESIS` (Adapter connections, directly to the SubApp interface).


## Technical Features

- **Configurable Default Values**: `rMI`/`rDEAD`/`rHYSTERESIS` have meaningful default values (`500.0`/`20.0`/`30.0`) that can be overridden via parameters when calling the function—similar to `u16ObjId` in the GreenWhiteBackground function blocks.

- **Reuse Instead of Repetition**: Replaces the manual duplication of three `initval_AR` instances, as was initially done in `Uebung_234_AX`/`Uebung_235_AX`.


## Application Scenarios

- Parameterization of `DualHysteresis_AR_AX`/`DualHysteresis_AR_A2X` when setpoint, dead zone, and hysteresis are required as fixed constants that can be overridden during the call.

- Consistent parameterization of several comparable exercises (e.g., `Uebung_234_AX` for hardware and `Uebung_235_AX` for VT) that should remain directly comparable with identical values.

## Summary

`HysteresisParams_AR` combines three fixed `AR` parameter constants for the hysteresis blocks into a single, parameterizable block.


---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

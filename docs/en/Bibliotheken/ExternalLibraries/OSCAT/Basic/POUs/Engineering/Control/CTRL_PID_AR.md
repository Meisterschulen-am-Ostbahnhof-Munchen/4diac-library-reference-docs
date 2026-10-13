# CTRL_PID_AR

![CTRL_PID_AR_network](./CTRL_PID_AR_network.svg)

* * * * * * * * * *

## Introduction

`CTRL_PID_AR` is an AR adapter wrapper around the OSCAT PID controller [`CTRL_PID`](./CTRL_PID.md) (`OSCAT::Basic::POUs::Engineering::Control::CTRL_PID`) and bridges between adapters and the OSCAT controller. Actual value, set point, manual switch, manual value, controller parameters as well as noise suppression, offset and limits are all implemented as adapter sockets - in the same order as the input variables of the wrapped `CTRL_PID`. No explicit `REQ` is needed, since each socket triggers `CTRL_PID.REQ` itself when a new value arrives.

![CTRL_PID_AR](CTRL_PID_AR.svg)

## Interface Structure

### **Event Inputs**

- **RST**: Reset of the integrator (passed through to `CTRL_PID.RST`)

### **Event Outputs**

*No direct event outputs - events run via the adapter plugs*

### **Data Inputs**

*No data inputs - all inputs are adapter sockets*

### **Data Outputs**

*No data outputs - all outputs are adapter plugs*

### **Adapters**

Sockets (inputs); `E1` triggers `CTRL_PID.REQ`, `D1` supplies the value:

- **AR_ACT** (`AR`): Actual value (REAL)
- **SET** (`AR`): Set point (REAL, dynamically calculated)
- **SUP** (`AR`): Noise suppression (REAL, dynamic)
- **OFS** (`AR`): Offset (REAL, dynamic)
- **M_I** (`AR`): Manual value (REAL, dynamic)
- **AX_MAN** (`AX`): Manual mode switch (BOOL/event, optional)
- **KP** (`AR`): Proportional gain (REAL, dynamic)
- **TN** (`AR`): Integral time (REAL, dynamic)
- **TV** (`AR`): Derivative time (REAL, dynamic)
- **LL** (`AR`): Lower limit (REAL, dynamic)
- **LH** (`AR`): Upper limit (REAL, dynamic)

Plugs (outputs):

- **AR_Y** (`AR`): Control output (REAL)
- **AR_DIFF** (`AR`): Control error (`DIFF = SET - ACT`, REAL)
- **AB_LIM** (`AB`): Limit active flag (`LIM`, BOOL)

## Function Blocks (FBs) Used

- **CTRL_PID**: [`OSCAT::Basic::POUs::Engineering::Control::CTRL_PID`](./CTRL_PID.md)
- **E_D_FF_ANY**: `iec61499::events::E_D_FF_ANY_3` - 3-fold data latch for `Y`, `DIFF`, `LIM`

## Program Flow and Connections

1. Every `E1` of the sockets `AR_ACT`, `SET`, `SUP`, `OFS`, `M_I`, `AX_MAN`, `KP`, `TN`, `TV`, `LL`, `LH` → `CTRL_PID.REQ`.
2. `D1` of the sockets → same-named input of `CTRL_PID` (`AR_ACT.D1` → `ACT`, `AX_MAN.D1` → `MAN`, otherwise identical names).
3. `RST` → `CTRL_PID.RST`.
4. `CTRL_PID.CNF` → `E_D_FF_ANY.CLK`; `CTRL_PID.Y`/`DIFF`/`LIM` → `E_D_FF_ANY.D1`/`D2`/`D3`.
5. `E_D_FF_ANY.EO` → `AR_Y.E1`, `AR_DIFF.E1`, `AB_LIM.E1`; `Q1`/`Q2`/`Q3` → `AR_Y.D1`, `AR_DIFF.D1`, `AB_LIM.D1`.

## Technical Details

- The latch (`E_D_FF_ANY`) triggers the outputs only after the controller's `CNF`; there is no external `CNF` event any more.
- Each input socket triggers a calculation; if several values change simultaneously, multiple `REQ` occur.
- The sockets are sorted in the order of the `InputVars` of `CTRL_PID` (version 1.4).

## Application Scenarios

- PID control in adapter networks, e.g. speed control of the pickup with a dynamically calculated set point and `AX_MAN` from an automatic/manual toggle

## 🛠️ Related Exercises

- [Uebung_153_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_153_AX/)

## Summary

`CTRL_PID_AR` connects the OSCAT PID controller entirely via AR/AX adapters, so that values flow only through adapter connections and no explicit `REQ` is needed.

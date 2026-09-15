# CTRL_PID_AR

## Introduction

The `CTRL_PID_AR` function block is an AR-adapter wrapper around the OSCAT PID controller `OSCAT::Basic::POUs::Engineering::Control::CTRL_PID`. It encapsulates the control logic within the IEC 61499 adapter architecture: Actual value (`AR_ACT`), Setpoint (`SET`), and optional manual mode toggle (`AX_MAN`) are provided as adapter sockets. Control output (`AR_Y`), control error (`AR_DIFF`), and limit active flag (`AB_LIM`) are provided as adapter plugs.

## Interface Structure

### **Event Inputs**

- `REQ`: Explicit execution request (forwarded to `CTRL_PID.REQ`)
- `RST`: Reset integrator (forwarded to `CTRL_PID.RST`)

### **Event Outputs**

- `CNF`: Execution confirmation (from `CTRL_PID.CNF`)

### **InputVars (Control Parameters)**

- `SUP` (REAL): Noise suppression (Default: 0.0)
- `OFS` (REAL): Offset (Default: 0.0)
- `M_I` (REAL): Manual input value (Default: 0.0)
- `KP` (REAL): Proportional gain (Default: 1.0)
- `TN` (REAL): Reset time (Default: 1.0)
- `TV` (REAL): Derivative time (Default: 1.0)
- `LL` (REAL): Lower limit (Default: -1000.0)
- `LH` (REAL): Upper limit (Default: 1000.0)

### **Sockets (Adapter Inputs)**

- `AR_ACT` (`adapter::types::unidirectional::AR`): Actual value (REAL)
- `SET` (`adapter::types::unidirectional::AR`): Setpoint (REAL, dynamically calculated or via `initval_AR`)
- `AX_MAN` (`adapter::types::unidirectional::AX`): Manual mode switch (optional)

### **Plugs (Adapter Outputs)**

- `AR_Y` (`adapter::types::unidirectional::AR`): Control output (REAL)
- `AR_DIFF` (`adapter::types::unidirectional::AR`): Control error (`DIFF = SET - ACT`, REAL)
- `AB_LIM` (`adapter::types::unidirectional::AB`): Limit active flag (BOOL)

## Functionality

Arrival of a new event on `AR_ACT.E1`, `SET.E1`, or `AX_MAN.E1` automatically triggers the underlying PID controller `CTRL_PID`. Alternatively, execution can be requested via the explicit `REQ` event input.

## Application Scenarios

- Closed-loop PID control of hydraulic valves, positions, or complex plants in ISOBUS and logiBUS applications.
- Connection to `initval_AR` for static setpoints or to dynamic setpoint generators.

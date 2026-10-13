# CTRL_PI_AR

![CTRL_PI_AR_network](./CTRL_PI_AR_network.svg)

* * * * * * * * * *

## Introduction

`CTRL_PI_AR` is an AR adapter wrapper around the OSCAT PI controller `OSCAT::Basic::POUs::Engineering::Control::CTRL_PI`. It bridges between adapters and OSCAT without either base library depending on the other. Actual value, set point, noise suppression, offset, manual value, manual mode switch, controller parameters and limits are all implemented as adapter sockets; an explicit `REQ` is no longer needed because every socket triggers the controller itself.

![CTRL_PI_AR](CTRL_PI_AR.svg)

## Interface Structure

### **Event Inputs**

- **RST**: reset of the integrator (passed on to `CTRL_PI.RST`)

### **Event Outputs**

The function block has no direct event outputs.

### **Data Inputs**

The function block has no direct data inputs.

### **Data Outputs**

The function block has no direct data outputs.

### **Adapters**

- **AR_ACT** (socket, `AR`): actual value (REAL)
- **SET** (socket, `AR`): set point (REAL, dynamically calculated)
- **SUP** (socket, `AR`): noise suppression (REAL, dynamic)
- **OFS** (socket, `AR`): offset (REAL, dynamic)
- **M_I** (socket, `AR`): manual value (REAL, dynamic)
- **AX_MAN** (socket, `AX`): switch to manual mode (optional)
- **KP** (socket, `AR`): proportional gain (REAL, dynamic)
- **KI** (socket, `AR`): integral gain (REAL, dynamic)
- **LL** (socket, `AR`): lower limit (REAL, dynamic)
- **LH** (socket, `AR`): upper limit (REAL, dynamic)
- **AR_Y** (plug, `AR`): control output (REAL)
- **AR_DIFF** (plug, `AR`): control error `DIFF = SET - ACT` (REAL)
- **AB_LIM** (plug, `AB`): flag 'limiting active' (BOOL)

## Functionality

The network consists of the controller `CTRL_PI` (`OSCAT::Basic::POUs::Engineering::Control::CTRL_PI`) and a change filter `E_D_FF_ANY` ([`iec61499::events::E_D_FF_ANY_3`](../../../../../../StandardLibraries/events/E_D_FF_ANY_3.md)):

- **Triggering**: the event `E1` of every socket (`AR_ACT`, `SET`, `SUP`, `OFS`, `M_I`, `AX_MAN`, `KP`, `KI`, `LL`, `LH`) goes to `CTRL_PI.REQ`. `RST` goes to `CTRL_PI.RST`.
- **Data**: `AR_ACT.D1` → `ACT`, `SET.D1` → `SET`, `SUP.D1` → `SUP`, `OFS.D1` → `OFS`, `M_I.D1` → `M_I`, `AX_MAN.D1` → `MAN`, `KP.D1` → `KP`, `KI.D1` → `KI`, `LL.D1` → `LL`, `LH.D1` → `LH` of the controller.
- **Output**: `CTRL_PI.CNF` → `E_D_FF_ANY.CLK`; `CTRL_PI.Y`, `CTRL_PI.DIFF` and `CTRL_PI.LIM` go to `E_D_FF_ANY.D1`..`D3`. Only if at least one of the three values changes does `E_D_FF_ANY.EO` emit the event to `AR_Y.E1`, `AR_DIFF.E1` and `AB_LIM.E1`, and `Q1`..`Q3` go to `AR_Y.D1`, `AR_DIFF.D1` and `AB_LIM.D1`.

## Technical Details

- The `CNF` event of the controller is now internal only (for the latch of `Y`/`DIFF`/`LIM`); there is no external `REQ`/`CNF` any more
- Every socket triggers the controller, so several controller runs are possible when several values arrive at the same time
- The sockets are sorted in the order of the `CTRL_PI` input variables: `AR_ACT`, `SET`, `SUP`, `OFS`, `M_I`, `AX_MAN`, `KP`, `KI`, `LL`, `LH`
- The adapter bridge resides in the library `OSCAT_adapter`, which depends on `adapter` and `OSCAT`; thus these two need not depend on each other

## Application Scenarios

- PI control in purely adapter-based networks, e.g. speed or rotational speed control with a dynamically calculated set point
- Controllers whose parameters (KP, KI) and limits can be changed at runtime via VT/OPC-UA

## 🛠️ Related Exercises

- [Uebung_152_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_152_AX/)

## Conclusion

`CTRL_PI_AR` makes the OSCAT PI controller adapter-capable: all inputs and outputs are AR/AX/AB adapters, every input triggers the calculation, and results are output only on change.

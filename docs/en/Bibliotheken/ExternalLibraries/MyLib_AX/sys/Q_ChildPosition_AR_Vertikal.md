# Q_ChildPosition_AR_Vertikal

![Q_ChildPosition_AR_Vertikal_network](./Q_ChildPosition_AR_Vertikal_network.svg)

* * * * * * * * * *

## Introduction

`Q_ChildPosition_AR_Vertikal` sets the position of a child object in the VT: the Y position comes via an AR adapter (`rPhys`, converted to INT), the X position is fixed at 0.

![Q_ChildPosition_AR_Vertikal](Q_ChildPosition_AR_Vertikal.svg)

## Interface Structure

### **Event Inputs**

- **INIT**: service initialisation (`EInit`)

### **Event Outputs**

- **INITO**: initialisation confirmation (`EInit`)
- **CNF**: confirmation of the requested service

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): Quality Input - TRUE enables the service
- **u16ObjId** (UINT): object ID of the child object (default `ID_NULL`)
- **u16ObjIdParent** (UINT): object ID of the parent object (default `ID_NULL`)
- **xScale** (BOOL): FALSE (default) = bypass, send X/Y position unchanged; TRUE = scale them by the DM/SKM factor for `u16ObjIdParent`

### **Data Outputs**

- **QO** (`BOOL`): Quality Output - TRUE if the service was executed successfully
- **STATUS** (STRING): service status, passed through from the internal `Q_ChildPosition`
- **s16OldXposition** (INT): old X position, passed through
- **s16OldYposition** (INT): old Y position, passed through
- **s16result** (INT): return value, passed through (see `Q_ChildPosition`)

### **Adapters**

- **rPhys** (socket, `adapter::types::unidirectional::AR`): physical REAL value (converted to INT and mapped to the Y position)

## Functionality

The network consists of three blocks:

- `AR_TO_AI` (`adapter::conversion::unidirectional::AR_TO_AI`): `rPhys` → `AR_TO_AI.AR_IN`; converts the REAL value into an INT adapter value.
- `initval_AI` (`adapter::types::unidirectional::AI::initval::initval_AI`, `INIT_VAL = 0`): supplies the fixed value 0 for the X position.
- `Q_ChildPosition_AI` (`isobus::UT::Q::Q_ChildPosition_AI`): `initval_AI.OUT` → `s16Xposition`, `AR_TO_AI.AI_OUT` → `s16Yposition`.

Initialisation: `INIT` → `initval_AI.INIT`, `initval_AI.INITO` → `Q_ChildPosition_AI.INIT`, `Q_ChildPosition_AI.INITO` → `INITO`. `Q_ChildPosition_AI.CNF` → `CNF`. `u16ObjId`, `u16ObjIdParent`, `xScale` and the four outputs go directly from/to `Q_ChildPosition_AI`.

`QI` is passed through to `Q_ChildPosition_AI.QI`, and `Q_ChildPosition_AI.QO` to `QO`.

## Technical Details

- Initialisation runs in series: first `initval_AI`, then `Q_ChildPosition_AI`
- `xScale` defaults to FALSE (bypass) here; in contrast, [`Q_ChildPosition_AR_Bargraph_LeftToRight`](Q_ChildPosition_AR_Bargraph_LeftToRight.md) defaults to TRUE
- The AR value is not scaled: `rPhys` is only converted to INT and used directly as the Y position

## Application Scenarios

- Vertical shifting of a VT object (e.g. marker) proportionally to a measured value, X position fixed at the left

## Conclusion

`Q_ChildPosition_AR_Vertikal` maps an AR value to the Y position of a child object via `AR_TO_AI` and holds the X position fixed at 0.

# Q_ChildPosition_AR_Horizontal

![Q_ChildPosition_AR_Horizontal_network](./Q_ChildPosition_AR_Horizontal_network.svg)

* * * * * * * * * *

## Introduction

`Q_ChildPosition_AR_Horizontal` changes the position of a VT child object (command *Change Child Position*): the **X position** arrives via an AR adapter, the **Y position is fixed at 0**. The `REAL` value is converted to `INT` and mapped to the X position.

![Q_ChildPosition_AR_Horizontal](Q_ChildPosition_AR_Horizontal.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): Initialization (to `initval_AI.INIT`)

### **Event Outputs**

- **INITO** (`EInit`): Initialization confirmation (from `Q_ChildPosition_AI.INITO`)
- **CNF**: Confirmation of the requested service (from `Q_ChildPosition_AI.CNF`)

### **Data Inputs**

- **u16ObjId** (`UINT`, default `ID_NULL`): Object ID of the child object
- **u16ObjIdParent** (`UINT`, default `ID_NULL`): Object ID of the parent object
- **xScale** (`BOOL`, default `FALSE`): `FALSE` = bypass, X/Y position are sent unchanged; `TRUE` = scale by the DM/SKM factor for `u16ObjIdParent`

### **Data Outputs**

- **STATUS** (`STRING`): Service status (passthrough from the inner `Q_ChildPosition`)
- **s16OldXposition** (`INT`): Old X position (passthrough)
- **s16OldYposition** (`INT`): Old Y position (passthrough)
- **s16result** (`INT`): Return value (passthrough, see `Q_ChildPosition`)

### **Adapters**

- **rPhys** (socket, `adapter::types::unidirectional::AR`): Physical `REAL` input value (converted to `INT` and mapped to the X position)

## Function Blocks (FBs) Used

- **AR_TO_AI**: `adapter::conversion::unidirectional::AR_TO_AI` - converts `REAL` to `INT`
- **initval_AI**: `adapter::types::unidirectional::AI::initval::initval_AI` (`INIT_VAL=0`) - provides the constant Y position 0
- **Q_ChildPosition_AI**: `isobus::UT::Q::Q_ChildPosition_AI` - sends the position change

## Program Flow and Connections

1. `rPhys` → `AR_TO_AI.AR_IN`; `AR_TO_AI.AI_OUT` → `Q_ChildPosition_AI.s16Xposition`.
2. `initval_AI.OUT` → `Q_ChildPosition_AI.s16Yposition` (constant 0).
3. `INIT` → `initval_AI.INIT`; `initval_AI.INITO` → `Q_ChildPosition_AI.INIT`; `Q_ChildPosition_AI.INITO` → `INITO`; `Q_ChildPosition_AI.CNF` → `CNF`.
4. `u16ObjId`, `u16ObjIdParent`, `xScale` → `Q_ChildPosition_AI`; status outputs from there to the outside.

## Technical Details

- Initialization runs in series: first `initval_AI`, then `Q_ChildPosition_AI`.
- Unlike `Q_ChildPosition_AR_Bargraph_BottomToTop` there is no scaling/mirroring; `xScale` defaults to `FALSE`.

## Application Scenarios

- Moving a VT object horizontally (e.g. pointer or bar) depending on a `REAL` value

## Summary

`Q_ChildPosition_AR_Horizontal` moves a VT child object horizontally based on an AR value at a fixed Y position of 0.

# Q_ChildPosition_AR_Bargraph_TopToBottom

![Q_ChildPosition_AR_Bargraph_TopToBottom_network](./Q_ChildPosition_AR_Bargraph_TopToBottom_network.svg)

* * * * * * * * * *

## Introduction

`Q_ChildPosition_AR_Bargraph_TopToBottom` is a **bar graph type 3 (top to bottom)**: the AR input is scaled with `ScalePx` and output as the vertical position of the child object in the VT (`X = 0`, `Y = value × ScalePx`). A larger value thus moves the child downwards. In contrast to [`Q_ChildPosition_AR_Bargraph_BottomToTop`](./Q_ChildPosition_AR_Bargraph_BottomToTop.md), no mirroring takes place.

![Q_ChildPosition_AR_Bargraph_TopToBottom](Q_ChildPosition_AR_Bargraph_TopToBottom.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): Initialization (to `QPos.INIT`)

### **Event Outputs**

- **INITO** (`EInit`): Initialization confirmation (from `QPos.INITO`)
- **CNF**: Confirmation of the requested service (from `QPos.CNF`)

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): quality input, TRUE enables the service
- **u16ObjId** (`UINT`, default `ID_NULL`): Object ID of the child object
- **u16ObjIdParent** (`UINT`, default `ID_NULL`): Object ID of the parent object
- **xScale** (`BOOL`, default `TRUE`): `FALSE` = bypass; `TRUE` = scale by the DM/SKM factor for `u16ObjIdParent`
- **ScalePx** (`REAL`, default `1.0`): Pixels per value unit (multiplication factor)

### **Data Outputs**

- **QO** (`BOOL`): quality output (from `QPos.QO`)
- **STATUS** (`STRING`): Service status (passthrough from the inner `Q_ChildPosition`)
- **s16OldXposition** (`INT`): Old X position (passthrough)
- **s16OldYposition** (`INT`): Old Y position (passthrough)
- **s16result** (`INT`): Return value (passthrough, see `Q_ChildPosition`)

### **Adapters**

- **rPhys** (socket, `adapter::types::unidirectional::AR`): Physical `REAL` input value (scaled and mapped to the Y position)

## Function Blocks (FBs) Used

- **AR_R_MUL**: SubApp `MyLib::sys::AR_R_MUL` - multiplies the value by `ScalePx` (`FACTOR`)
- **QPos**: SubApp `MyLib::sys::Q_ChildPosition_AR_Vertikal` - sets the vertical child position

## Program Flow and Connections

1. `rPhys` → `AR_R_MUL.IN1`; `ScalePx` → `AR_R_MUL.FACTOR`.
2. `AR_R_MUL.OUT` → `QPos.rPhys`.
3. `INIT` → `QPos.INIT`; `QPos.INITO` → `INITO`; `QPos.CNF` → `CNF`; `QI` → `QPos.QI`; `QPos.QO` → `QO`.
4. `u16ObjId`, `u16ObjIdParent`, `xScale` → `QPos`; `STATUS`, `s16OldXposition`, `s16OldYposition`, `s16result` from `QPos` to the outside.

## Technical Details

- No mirroring: since the VT Y coordinate grows from top to bottom, a larger value corresponds directly to a position further down.
- `xScale` defaults to `TRUE` here.
- Version 1.0 (2026-09-27): `AR_R_MUL` + `Q_ChildPosition_AR_Vertikal`

## Application Scenarios

- Vertical bar/display that grows from top to bottom

## Summary

`Q_ChildPosition_AR_Bargraph_TopToBottom` maps an AR value directly to the vertical position of a VT child object via scaling.

# Q_ChildPosition_AR_Bargraph_BottomToTop

![Q_ChildPosition_AR_Bargraph_BottomToTop_network](./Q_ChildPosition_AR_Bargraph_BottomToTop_network.svg)

* * * * * * * * * *

## Introduction

`Q_ChildPosition_AR_Bargraph_BottomToTop` is a **bar graph type 1 (bottom to top)**: the AR input is scaled with `ScalePx`, mirrored against `LengthPx` and output as the vertical position of the child object in the VT (`X = 0`, `Y = LengthPx - (value × ScalePx)`). A larger value thus moves the child upwards.

![Q_ChildPosition_AR_Bargraph_BottomToTop](Q_ChildPosition_AR_Bargraph_BottomToTop.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): Initialization (to `QPos.INIT`)

### **Event Outputs**

- **INITO** (`EInit`): Initialization confirmation (from `QPos.INITO`)
- **CNF**: Confirmation of the requested service (from `QPos.CNF`)

### **Data Inputs**

- **u16ObjId** (`UINT`, default `ID_NULL`): Object ID of the child object
- **u16ObjIdParent** (`UINT`, default `ID_NULL`): Object ID of the parent object
- **xScale** (`BOOL`, default `TRUE`): `FALSE` = bypass; `TRUE` = scale by the DM/SKM factor for `u16ObjIdParent`
- **ScalePx** (`REAL`, default `1.0`): Pixels per value unit (multiplication factor)
- **LengthPx** (`REAL`, default `100.0`): Total length of the bar graph in pixels (for mirroring)

### **Data Outputs**

- **STATUS** (`STRING`): Service status (passthrough from the inner `Q_ChildPosition`)
- **s16OldXposition** (`INT`): Old X position (passthrough)
- **s16OldYposition** (`INT`): Old Y position (passthrough)
- **s16result** (`INT`): Return value (passthrough, see `Q_ChildPosition`)

### **Adapters**

- **rPhys** (socket, `adapter::types::unidirectional::AR`): Physical `REAL` input value (scaled, mirrored and mapped to the Y position)

## Function Blocks (FBs) Used

- **AR_R_MUL**: SubApp `MyLib::sys::AR_R_MUL` - multiplies the value by `ScalePx` (`FACTOR`)
- **SubFlip**: SubApp `MyLib::sys::R_AR_SUB` - subtracts from the `MINUEND` (`LengthPx`) and thereby mirrors
- **QPos**: SubApp `MyLib::sys::Q_ChildPosition_AR_Vertikal` - sets the vertical child position

## Program Flow and Connections

1. `rPhys` → `AR_R_MUL.IN1`; `ScalePx` → `AR_R_MUL.FACTOR`.
2. `AR_R_MUL.OUT` → `SubFlip.IN`; `LengthPx` → `SubFlip.MINUEND`.
3. `SubFlip.OUT` → `QPos.rPhys`.
4. `INIT` → `QPos.INIT`; `QPos.INITO` → `INITO`; `QPos.CNF` → `CNF`.
5. `u16ObjId`, `u16ObjIdParent`, `xScale` → `QPos`; `STATUS`, `s16OldXposition`, `s16OldYposition`, `s16result` from `QPos` to the outside.

## Technical Details

- The mirroring is necessary because the VT Y coordinate grows from top to bottom.
- `xScale` defaults to `TRUE` here (whereas to `FALSE` in `Q_ChildPosition_AR_Horizontal`).

## Application Scenarios

- Vertical bar/level display that grows from bottom to top

## Summary

`Q_ChildPosition_AR_Bargraph_BottomToTop` maps an AR value to the vertical position of a VT child object via scaling and mirroring.

# ReportScrollOffset

* * * * * * * * * *

## Introduction

`ReportScrollOffset` is a helper Service Interface block from package `isobus::UT::Q::helpers`. It sits inside the [ScrollFS](../ScrollFS.md) FBNetwork structure (parallel to `MoveList` and `ListY`) and reports the current scroll offset (row index `i32Pos` × row height `i32RowHeight` in pixels) for container `u16ContainerId` to the ECU visibility gate (`VtMaskVisibility`).

## Interface Structure

### **Event Inputs**

- `INIT`: Service interface initialization (`With u16ContainerId`).
- `REQ`: Reports a position change (`With i32Pos`, `With i32RowHeight`).

### **Event Outputs**

- `INITO`: Initialization confirmation.
- `CNF`: Position report confirmation.

### **Data Inputs**

- `u16ContainerId` (UINT): Object ID of the `*_Scrolling_Content` container.
- `i32Pos` (DINT): Current row position (0…i32PosMax).
- `i32RowHeight` (DINT): Row height in pixels for pixel offset conversion.

### **Data Outputs**

No data outputs.

### **Adapters**

No adapters available.

## Functionality

1. At `INIT`, latches the container ID `u16ContainerId`.
2. On each `REQ` event, calculates `PixelOffset := i32Pos * i32RowHeight` and forwards it to the ECU visibility gate (`VtMaskVisibility_OnScroll`).
3. The ECU gate uses this information to restrict VT updates (e.g. `Q_*` commands) to rows currently visible on screen.

## See Also

- [ScrollFS](../ScrollFS.md)
- [ScrollFS_PHYS_Softkey](../ScrollFS_PHYS_Softkey.md)
- [ScrollFS_PHYS_Button](../ScrollFS_PHYS_Button.md)

# ScrollObjectPool_S

* * * * * * * * * *

## Introduction

Description from the comment of the type: ISOBUS UT scroll list object pool properties (list + scrollbar geometry)

Package: `isobus::utils::scroll`

## Interface structure

### **Fields**

-   **u16ListParentId** (Type: `UINT`): Object ID of the list viewport container (Change Child Position parent) (Default: `IDs::ID_NULL`)
-   **u16ListContentId** (Type: `UINT`): Object ID of the moved list content container (Change Child Position child) (Default: `IDs::ID_NULL`)
-   **i32RowHeight** (Type: `DINT`): Row height in pixels (Default: `0`)
-   **u16BarParentId** (Type: `UINT`): Object ID of the scrollbar viewport container (Change Child Position parent) (Default: `IDs::ID_NULL`)
-   **u16BarContentId** (Type: `UINT`): Object ID of the moved scrollbar indicator content container (Change Child Position child) (Default: `IDs::ID_NULL`)
-   **i32BarBaseOffset** (Type: `DINT`): BarContent.Top at position 0 (negative of the indicator's fixed internal Top) (Default: `0`)
-   **i32BarTravel** (Type: `DINT`): Total pixels the indicator travels: window height minus indicator height (Default: `0`)
-   **i32PosMax** (Type: `DINT`): Upper bound of the scroll position: row count minus visible rows (Default: `0`)
-   **i32Step** (Type: `DINT`): Step size for the fast (page) scroll events (Default: `1`)

Note: This page was generated from the type definition (`ScrollObjectPool_S.dtp`) and only shows the interface. A detailed description of the behavior is still missing.

# PositionMarker_S

* * * * * * * * * *

## Introduction

Description from the comment of the type: ISOBUS UT position-marker object pool properties (Change Child Position child/parent + travel bounds + center offset)

Package: `isobus::utils::childposition`

## Interface structure

### **Fields**

-   **u16ChildId** (Type: `UINT`): Object ID of the moved VT object (Change Child Position child, e.g. a CPolygon) (Default: `IDs::ID_NULL`)
-   **u16ParentId** (Type: `UINT`): Object ID of the parent VT container the child moves within (Change Child Position parent) (Default: `IDs::ID_NULL`)
-   **r32MinPos** (Type: `REAL`): Lower bound of the child's valid X-travel inside the parent, in raw pixels (normally 0.0) (Default: `0.0`)
-   **r32MaxPos** (Type: `REAL`): Upper bound of the child's valid X-travel inside the parent, in raw pixels = ContainerWidth - ChildBoundingBoxWidth (Default: `0.0`)
-   **r32Center** (Type: `REAL`): Offset added to the incoming value before clamping - the raw pixel X position that corresponds to value = 0.0 (Default: `0.0`)
-   **s16YPosition** (Type: `INT`): Fixed Y position passed straight through to Q_ChildPosition.s16Yposition (this FB drives only the X axis) (Default: `0`)

Note: This page was generated from the type definition (`PositionMarker_S.dtp`) and only shows the interface. A detailed description of the behavior is still missing.

# Instance Uniqueness of Q_* Function Blocks

Each `Q_*` function block maintains its own independent value/pending buffer (`var_qPending`, `var_*OldValue`, see `sendXxx()`/`resetPendingState()`). If two instances of the SAME class are connected to the same target (Object ID, Mask ID, etc.), they operate independently without mutual awareness, leading to competing VT bus commands. Therefore, each block requires an instance uniqueness criterion defining what may exist at most once.

Since commit `792e35673` ("Detect duplicate object-ID claims across instances of the same FB class"), an enforcement mechanism exists: `CQ_VTCommandBase::mTargetObjectId` (+ optional `mTargetSubId` for composite keys, see `Q_Attribute` below) + `IsObjectIdAlreadyClaimed(objId, subId = 0xFFFF)` (`CQ_VTCommandBase.h`/`.cpp`). A second instance encountering an already claimed (Object ID[, Sub-ID]) pair upon `INIT` is deactivated with `STATUS = "This objID is already in use"`.

## Criterion: One Instance PER OBJECT ID (Enforced)

The target is a variable VT Object ID (`u16ObjId`, set during `INIT` via `var_u16ObjId` and validated against class-specific ID ranges). Two instances with the same Object ID would compete independently over the same object state.

| Function Block | Target ID Definition |
|---|---|
| `Q_Attribute` | **Pair** (Object ID, Attribute ID) - `mTargetObjectId=u16ObjId`, `mTargetSubId=u8IdAttribute`. Two instances targeting the same object but different attributes are valid; only duplicate pairs collide. |
| `Q_BackgroundColour` | Object with background color attribute |
| `Q_ChangeObjectLabel` | Labelable object |
| `Q_ChangePolygonPoint` | Polygon object |
| `Q_ChangePolygonScale` | Polygon object |
| `Q_ChildLocation` | Child object (Child Location) |
| `Q_ChildPosition` | Child object (Child Position) |
| `Q_EndPoint` | Line object |
| `Q_FillAttributes` | Object with fill attributes |
| `Q_FontAttributes` | Object with font attributes |
| `Q_GraphicsContext` | Graphics Context object |
| `Q_LineAttributes` | Object with line attributes |
| `Q_ListItem` | **Pair** (List Object ID, List Index) - `mTargetObjectId=u16ObjId`, `mTargetSubId=u8ListIndex`. Read at `INIT` via `With Var="u8ListIndex"` — claimed during `INIT` in `iso_init()`. |
| `Q_NumericValue` | Numeric Value object. (ObjectPointer exception allows co-existence with `Q_NumericValueAux`). |
| `Q_ObjEnableDisable` | Enableable/disableable object |
| `Q_ObjHideShow` | Container object (3000–3999) |
| `Q_ObjSelectInput` | Input-selectable object |
| `Q_Size` | Resizable object |
| `Q_StringValue` | String Value object |

## Criterion: One Instance PER OBJECT ID (Not Yet Enforced)

| Function Block | Target ID Definition | Notes |
|---|---|---|
| `Q_NumericValueAux` | AUX VT Object | Additional gate (`VtAuxAssignment`) |
| `Q_BackgroundColourAux` | AuxFunction Object | Unique per class (`getFBTypeId()`) |
| `Q_ExecuteMacro` | Macro object | |
| `Q_ExecuteExtendedMacro` | Macro object | |
| `Q_LockUnlockMask` | Mask Object (`u16MaskId`, read at `INIT`) | Target is a mask (`u16MaskId`) |
| `Q_Priority` | Alarm Mask object | |

## Criterion: Exactly ONE Instance Program-Wide (Singleton)

| Function Block | Real Target (Parameter at REQ) | Status |
|---|---|---|
| `Q_ActiveMask` | `u16NewMaskId` (Fixed target ID = 0) | **Enforced** |
| `Q_SelectActiveWorkingSet` | Working Set Name | Open gap |
| `Q_SelectColourMap` | Colour Map Object ID | Open gap |
| `Q_SetAudioVolume` | Volume setting | Open gap |
| `Q_SoftKeyMask` | Data Mask / SoftKey Mask IDs | Open gap |
| `Q_CtrlAudioSignal` | Signal parameters | Open gap |

## No Criterion Needed (Stateless)

| Function Block | Reason |
|---|---|
| `Q_ESC` | No Object ID or old value buffering. Direct transmission upon `REQ`. |

## Separate I_* Module

`I_GetAttribute` (`isobus_UT/src/isobus/UT/I/`) requires pair uniqueness over (`u16ObjId`, `u8IdAttribute`).

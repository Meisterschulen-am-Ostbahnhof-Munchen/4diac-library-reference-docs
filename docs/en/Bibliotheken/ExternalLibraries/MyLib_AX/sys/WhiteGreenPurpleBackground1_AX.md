# WhiteGreenPurpleBackground1_AX

![WhiteGreenPurpleBackground1_AX_network](./WhiteGreenPurpleBackground1_AX_network.svg)

* * * * * * * * * *

## Introduction

`WhiteGreenPurpleBackground1_AX` sets the background colour for **one object** (`AX` adapter) with three states:

- `DI1=FALSE` → **white** (not fulfilled)
- `DI1=TRUE` → **green** (fulfilled)
- `DI2=TRUE` → **purple** (override, takes precedence over `DI1`)

It is used, for example, for precondition badges (saw/belts/PTO) of the pick-up automation. The block is built analogously to [`GreenWhiteBackground1_AX`](./GreenWhiteBackground1_AX.md).

![WhiteGreenPurpleBackground1_AX](WhiteGreenPurpleBackground1_AX.svg)

## Function Blocks (FBs) Used

### Sub-blocks: WhiteGreenPurpleBackground1_AX

- **Type**: SubAppType
- **Internal blocks used**:
    - **F_SEL_Condition**: SubApp [`MyLib::sys::AUS_AX_SEL_AUS_2_VAL`](./AUS_AX_SEL_AUS_2_VAL.md) with `val0=COLOR_WHITE`, `val1=COLOR_GREEN` - selects white or green based on `DI1`
    - **F_SEL_Override**: `adapter::iec61131::selection::AUS_AX_SEL_AUS` - selects between the condition result (`IN0`) and purple (`IN1`) based on `DI2`
    - **initval_AUS**: `adapter::types::unidirectional::AUS::initval::initval_AUS` with `INIT_VAL=COLOR_PURPLE`
    - **Q_BackgroundColour**: `isobus::UT::Q::Q_BackgroundColour_AUS` - writes the colour to the VT object `u16ObjId`
- **Imports**: `ID_NULL`, `COLOR_GREEN`, `COLOR_WHITE`, `COLOR_PURPLE`

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): Initialization request (→ `Q_BackgroundColour.INIT`)

Further events arrive via the adapters `DI1` and `DI2`.

### **Event Outputs**

- **INITO** (`EInit`): Initialization confirmation (from `Q_BackgroundColour.INITO`)
- **CNF**: Execution confirmation (from `Q_BackgroundColour.CNF`)

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): Quality Input - `TRUE` enables the service (→ `Q_BackgroundColour.QI`)
- **u16ObjId** (`UINT`, default `ID_NULL`): Object ID of softkey/button/output text

### **Data Outputs**

- **QO** (`BOOL`): Quality Output - `TRUE` if the service executed successfully (from `Q_BackgroundColour.QO`)
- **STATUS_1** (`STRING`): Service status (from `Q_BackgroundColour.STATUS`)
- **result_1** (`INT`): Return value (from `Q_BackgroundColour.s16result`)

### **Adapters**

- **DI1** (socket, `adapter::types::unidirectional::AX`): Condition fulfilled (FALSE = white, TRUE = green), without precedence, see `DI2`
- **DI2** (socket, `adapter::types::unidirectional::AX`): Override active (TRUE = purple), takes precedence over `DI1`
- **u8OldColour_1** (plug, `adapter::types::unidirectional::AUS`): Old colour value

## Program Flow and Connections

- `DI1` → `F_SEL_Condition.G`
- `DI2` → `F_SEL_Override.G`
- `F_SEL_Condition.OUT` → `F_SEL_Override.IN0`; `initval_AUS.OUT` → `F_SEL_Override.IN1`
- `F_SEL_Override.OUT` → `Q_BackgroundColour.u8Colour`
- `u16ObjId` → `Q_BackgroundColour.u16ObjId`
- `INIT` → `Q_BackgroundColour.INIT`; `Q_BackgroundColour.INITO` → `INITO`
- `QI` → `Q_BackgroundColour.QI`; `Q_BackgroundColour.QO` → `QO`
- `Q_BackgroundColour.u8OldColour` → `u8OldColour_1`; `.STATUS` → `STATUS_1`; `.s16result` → `result_1`; `.CNF` → `CNF`

## Technical Details

- Two nested selectors (not a single `AX_SEL`), since a single `AX_SEL` re-evaluates only when its **own** `G` changes and the second stage would otherwise not follow up on a change of `DI1`
- Both selectors are re-evaluated in series via a common trigger (`E_MERGE_2` from `DI1.E1`/`DI2.E1`), so that a change of `DI1` **or** `DI2` always yields the complete, correct colour
- Version 1.0 (2026-10-01)

## State Overview

| `DI1` | `DI2` | Colour |
| :--- | :--- | :--- |
| FALSE | FALSE | White (`COLOR_WHITE`) |
| TRUE | FALSE | Green (`COLOR_GREEN`) |
| any | TRUE | Purple (`COLOR_PURPLE`) |

## Application Scenarios

- Precondition badges (saw, belts, PTO) of a pick-up automation with override indication

## Summary

`WhiteGreenPurpleBackground1_AX` colours a VT object white, green or purple, where the override `DI2` takes precedence over the condition `DI1`.

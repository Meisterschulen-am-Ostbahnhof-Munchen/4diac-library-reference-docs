# ILOCK_CONFLICT_TRIP_PROTECT

![ILOCK_CONFLICT_TRIP_PROTECT](./ILOCK_CONFLICT_TRIP_PROTECT.svg)

* * * * * * * * * *

## Introduction

The function block `ILOCK_CONFLICT_TRIP_PROTECT` extends `ILOCK_CONFLICT_TRIP` with a configurable protection dead time (`DT_PROTECT`), analogous to how `ILOCK_BLOCK_PROTECT` extends the behavior of `ILOCK_BLOCK`. It still prioritizes the first active input, triggers a trip state when both directions are activated simultaneously, and requires an explicit `EI_RESET` thereafter. What's new is that after the active input is released, the dead time `DT_PROTECT` must elapse before the function block re-evaluates the current input signals – only then is the next direction (or, if applicable, a trip) taken.

## Interface Structure

### **Event Inputs**

| Name | Carrying Data | Description |
| ---------- | --------------------------- | ----------------------------------------------------------------------------- |
| `EI_UP` | `DI_UP`, `DT_PROTECT` | Event for the up/forward direction. |
| `EI_DOWN` | `DI_DOWN`, `DT_PROTECT` | Down/Backward Direction Event. |
| `EI_RESET` | `DI_UP`, `DI_DOWN` | Reset Trip State; only effective if both data inputs are FALSE. |

### **Event Outputs**

| Name | Carrying Data | Description |
| ---------- | ------------------ | ------------------------------------------ |
| `EO_UP` | `DO_UP` | Triggered when the UP direction is enabled or disabled. |
| `EO_DOWN` | `DO_DOWN` | Triggered when the DOWN direction is enabled or disabled. |
| `EO_TRIP` | `DO_TRIP` | Sent in the TRIP state, it indicates the trip status. |

### **Data Inputs**

- `DI_UP` (BOOL) – TRUE = forward/upward/right/clockwise.

- `DI_DOWN` (BOOL) – TRUE = backward/downward/left/counterclockwise.

- `DT_PROTECT` (TIME, initial value `T#50ms`) – Protection dead time that must elapse after the active input is released before it can be re-evaluated.


### **Data Outputs**

- `DO_UP` (BOOL) – Signals the active UP direction.

- `DO_DOWN` (BOOL) – Signals the active DOWN direction.

- `DO_TRIP` (BOOL) – Signals the trip/conflict state.

### **Adapters**

| Adapter | Type | Direction | Description |
| --------- | ------------------------------ | -------- | -------------------------------------------------------------------------------------------------- |
| `timeOut` | `iec61499::events::ATimeOut` | Plug | Timer adapter for protection time. The function block sets `timeOut.DT` and starts it via `timeOut.START`; `timeOut.TimeOut` signals the execution. |

## Functionality

The function block operates as a finite state machine (ECC) with seven states:

1. **STOP** – Idle state, all outputs FALSE. At `EI_UP` with `DI_UP AND NOT DI_DOWN` → **UP**; at `EI_DOWN` with `DI_DOWN AND NOT DI_UP` → **DOWN**; if both data signals are TRUE at `EI_UP` or `EI_DOWN` → immediate **TRIP**.

2. **UP** – `DO_UP = TRUE`. At `EI_UP[NOT DI_UP]` → **UP_STOP**; at `EI_DOWN[DI_DOWN]` (conflict during active UP) → immediate **TRIP**.

3. **DOWN** – `DO_DOWN = TRUE`. At `EI_DOWN[NOT DI_DOWN]` → **DOWN_STOP**; at `EI_UP[DI_UP]` (conflict during active DOWN) → immediate **TRIP**.

4. **UP_STOP** / **DOWN_STOP** – Transition states after the active input is released. The algorithm `STOP` sets all outputs to FALSE, transfers `DT_PROTECT` to `timeOut.DT`, and starts the timer (`timeOut.START`). Upon expiration (`timeOut.TimeOut`) → **EVAL**.

5. **EVAL** – No separate algorithm. The current values of `DI_UP`/`DI_DOWN` are re-evaluated: only `DI_UP` TRUE → **UP**; only `DI_DOWN` TRUE → **DOWN**; both FALSE → **STOP**; both TRUE → **TRIP**.

6. **TRIP** – `DO_TRIP = TRUE`, all other outputs FALSE. Exit only via `EI_RESET` if `NOT DI_UP AND NOT DI_DOWN` → **STOP**.

Trip detection itself still occurs immediately and independently of the dead time: A conflict during `STOP`, `UP`, or `DOWN` always directly triggers `TRIP`. The dead time `DT_PROTECT` only affects the period between the release of an active input and the acquisition of the next direction.


Trip detection itself continues to occur immediately and independently of the dead time: A conflict during `STOP`, `UP`, or `DOWN` always directly triggers `TRIP`. The dead time `DT_PROTECT` is only effective between the release of an active input and the acquisition of the next direction.


## Technical Features

- **Combination of two patterns:** This function block combines the trip-on-conflict logic of `ILOCK_CONFLICT_TRIP` with the dead-time logic of `ILOCK_BLOCK_PROTECT`.

- **Immediate trip, delayed release:** Conflicts are detected without delay; only the return to a new valid state after release is delayed by `DT_PROTECT`.

- **Reset condition:** `EI_RESET` only takes effect if both data inputs are inactive – a reset while a conflict persists has no effect.

- **Timer restart in every intermediate stop state:** `timeOut.DT` is reused from `DT_PROTECT` upon each entry into `UP_STOP`/`DOWN_STOP`, so that any parameter change takes effect immediately in the next delay.

## State Overview

| State | DO_UP | DO_DOWN | DO_TRIP | Description |
| ----------- | ----- | ------- | ------- | -------------------------------------------------------- |
| `STOP` | FALSE | FALSE | FALSE | Idle state, no direction active. |
| `UP` | TRUE | FALSE | FALSE | Up direction active. |
| `DOWN` | FALSE | TRUE | FALSE | Downward direction active. |
| `UP_STOP` | FALSE | FALSE | FALSE | Waiting for `DT_PROTECT` to expire after UP is released. |
| `DOWN_STOP` | FALSE | FALSE | FALSE | Waiting for `DT_PROTECT` to expire after DOWN is released. |
| `EVAL` | – | – | – | No algorithm; decides the next state based on current inputs. |
| `TRIP` | FALSE | FALSE | TRUE | Conflict/Trip, requires `EI_RESET`. |

## Application Scenarios

- **Drives with Overrun:** When a mechanical or hydraulic overrun time must be observed after releasing a travel command before switching to the opposite direction is safe.

- **Safety-Oriented Interlock with Acknowledgement Required:** Applications where a simultaneous command in both directions is considered an error and must be explicitly acknowledged.

- **Valve or Flap Controls:** Protection against pressure surges or mechanical overload through a minimum pause between direction changes.

## Comparison with Similar Function Blocks

Compared to `ILOCK_CONFLICT_TRIP`, this function block adds the states `UP_STOP`, `DOWN_STOP`, and `EVAL`, as well as the `timeOut` adapter – the trip logic itself remains identical. It differs from `ILOCK_BLOCK_PROTECT` in that a simultaneous command in both directions is not ignored, but rather treated as an explicit error condition (`TRIP`) that requires `EI_RESET`.


## Conclusion

`ILOCK_CONFLICT_TRIP_PROTECT` combines the clear error detection of `ILOCK_CONFLICT_TRIP` with the protection dead time of `ILOCK_BLOCK_PROTECT`. It is suitable for applications that require both strict conflict detection with mandatory acknowledgment and a minimum pause between direction changes.

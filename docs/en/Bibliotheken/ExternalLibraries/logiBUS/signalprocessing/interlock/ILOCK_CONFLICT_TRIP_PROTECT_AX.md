# ILOCK_CONFLICT_TRIP_PROTECT_AX

![ILOCK_CONFLICT_TRIP_PROTECT_AX](./ILOCK_CONFLICT_TRIP_PROTECT_AX.svg)

* * * * * * * * * *

## Introduction

The function block `ILOCK_CONFLICT_TRIP_PROTECT_AX` is the adapter version of `ILOCK_CONFLICT_TRIP_PROTECT`: It combines the trip-on-conflict logic of `ILOCK_CONFLICT_TRIP_AX` with the protection dead time of `ILOCK_BLOCK_PROTECT_AX`. The first active input is prioritized; simultaneous activation of both directions immediately triggers a trip, which can only be reset via `EI_RESET`. After the active input is released, the block additionally waits for the configurable time `DT_PROTECT` before re-evaluating the inputs. The process data (`UP_IN`/`DOWN_IN`/`UP_OUT`/`DOWN_OUT`/`TRIP_OUT`) is routed via adapters of type `unidirectional::AX`; the protection-time timer additionally uses the adapter `iec61499::events::ATimeOut` (`timeOut`).

## Interface Structure

### **Event Inputs**

| Name | Carrying Data | Description |
| ---------- | ------------------- | ------------------------------------------------------------------------------ |
| `EI_RESET` | – | Resets the trip state; only effective if `UP_IN.D1` and `DOWN_IN.D1` are both FALSE. |
| `UPDATE` | `DT_PROTECT` | Updates the protection time `DT_PROTECT` at runtime without changing the current state. |


### **Event Outputs**

No direct event outputs. State changes are signaled via the events of the output adapters (plugs):

- `UP_OUT.E1`, `DOWN_OUT.E1`, `TRIP_OUT.E1`

### **Data Inputs**

No direct data inputs. Provided via the socket adapters:

- `UP_IN.D1` (BOOL) – Upward direction activation.

- `DOWN_IN.D1` (BOOL) – Downward direction activation.

- `DT_PROTECT` (TIME, initial value `T#50ms`) – Protection dead time after releasing the active input.


### **Data Outputs**

No direct data outputs. Provided via the plug adapters:

- `UP_OUT.D1` (BOOL) – Up signal.

- `DOWN_OUT.D1` (BOOL) – Down signal.

- `TRIP_OUT.D1` (BOOL) – Trip signal.

### **Adapters**

**Sockets (Inputs)**

| Adapter | Type | Description |
| --------- | ------------------------------------- | ----------------------------------------- |
| `UP_IN` | `adapter::types::unidirectional::AX` | Up input. |
| `DOWN_IN` | `adapter::types::unidirectional::AX` | Down-direction input. |

**Plugs (Outputs)**

| Adapter | Type | Description |
| ---------- | ------------------------------------- | ---------------------------------- |
| `UP_OUT` | `adapter::types::unidirectional::AX` | Up-direction output. |
| `DOWN_OUT` | `adapter::types::unidirectional::AX` | Down-direction output. |
| `TRIP_OUT` | `adapter::types::unidirectional::AX` | Trip state output. |
| `timeOut` | `iec61499::events::ATimeOut` | Timer adapter for the protection time; the module sets `timeOut.DT` and starts it via `timeOut.START`. |

## Functionality

The circuit is structurally identical to `ILOCK_CONFLICT_TRIP_PROTECT`, except that all signals are routed via adapters:

1. **STOP** – Idle state. At `UP_IN.E1[UP_IN.D1 AND NOT DOWN_IN.D1]` → **UP**; at `DOWN_IN.E1[DOWN_IN.D1 AND NOT UP_IN.D1]` → **DOWN**; if both data signals are TRUE at either event → immediate **TRIP**.

2. **UP** – `UP_OUT.D1 = TRUE`. At `UP_IN.E1[NOT UP_IN.D1]` → **UP_STOP**; at `DOWN_IN.E1[DOWN_IN.D1]` (conflict) → immediate **TRIP**.

3. **DOWN** – `DOWN_OUT.D1 = TRUE`. At `DOWN_IN.E1[NOT DOWN_IN.D1]` → **DOWN_STOP**; at `UP_IN.E1[UP_IN.D1]` (conflict) → immediate **TRIP**.

4. **UP_STOP** / **DOWN_STOP** – The algorithm `STOP` sets all adapter outputs to FALSE, transmits `DT_PROTECT` to `timeOut.DT`, and starts the timer. At `timeOut.TimeOut` → **EVAL**.

5. **EVAL** – No separate algorithm. Re-evaluation: only `UP_IN.D1` TRUE → **UP**; Only `DOWN_IN.D1` is TRUE → **DOWN**; both are FALSE → **STOP**; both are TRUE → **TRIP**.

6. **TRIP** – `TRIP_OUT.D1 = TRUE`. Exit only via `EI_RESET[NOT UP_IN.D1 AND NOT DOWN_IN.D1]` → **STOP**.

Additionally, in states `STOP`, `UP`, and `DOWN`, there is a self-loop transition to the event `UPDATE`, which does not leave the respective state and serves solely to acquire a new `DT_PROTECT` value.


## Technical Features

- **Pure Adapter Interface:** All process data is routed via `unidirectional::AX` adapters (the protection-time timer additionally via an `iec61499::events::ATimeOut` adapter). There are no traditional event/data ports except for `EI_RESET`, `UPDATE`, and `DT_PROTECT`.

- **Dynamic Dead Time:** The `UPDATE` event allows `DT_PROTECT` to be modified at runtime without exiting the automaton. The new value takes effect upon the next entry into `UP_STOP`/`DOWN_STOP`.

- **Immediate Trip in Case of Conflict:** As with the non-adapter variant, a simultaneous command in both directions is detected without delay.


- **Reset Condition:** `EI_RESET` only takes effect if both input adapters report `D1 = FALSE`.

## State Overview

| State | UP_OUT.D1 | DOWN_OUT.D1 | TRIP_OUT.D1 | Description |
| ----------- | --------- | ----------- | ----------- | -------------------------------------------------------- |
| `STOP` | FALSE | FALSE | FALSE | Idle state, no direction active. |
| `UP` | TRUE | FALSE | FALSE | Up direction active. |
| `DOWN` | FALSE | TRUE | FALSE | Downward direction active. |
| `UP_STOP` | FALSE | FALSE | FALSE | Waiting for `DT_PROTECT` to expire after UP is released. |
| `DOWN_STOP` | FALSE | FALSE | FALSE | Waiting for `DT_PROTECT` to expire after DOWN is released. |
| `EVAL` | – | – | – | No algorithm; decides on the next state. |
| `TRIP` | FALSE | FALSE | TRUE | Conflict/Trip, requires `EI_RESET`. |


## Application Scenarios

- **Adapter-Based Drive Controls:** Modular systems where direction signals are already routed through the system via `unidirectional::AX` adapters.

- **Safety-Oriented Interlocking with Overrun Time:** Applications requiring both strict conflict detection with mandatory acknowledgement and a minimum pause between direction changes.

- **Runtime Parameterization:** Systems where the protection time must be adjusted via `UPDATE` depending on the operating state (e.g., temperature, load).


## Comparison with similar building blocks

Compared to `ILOCK_CONFLICT_TRIP_AX`, this building block adds the states `UP_STOP`, `DOWN_STOP`, `EVAL`, as well as the `timeOut` adapter and the `UPDATE` event. Compared to `ILOCK_BLOCK_PROTECT_AX`, it differs in that a simultaneous command in both directions is not silently ignored, but rather treated as an explicit `TRIP` state, which requires an `EI_RESET` response. Compared to the non-adapter variant `ILOCK_CONFLICT_TRIP_PROTECT`, the interface is fully adapter-based, which facilitates integration into modular, adapter-oriented systems.

## Conclusion

`ILOCK_CONFLICT_TRIP_PROTECT_AX` combines adapter-based conflict detection with mandatory acknowledgment and a configurable protection dead time in a single module. It is suitable for modular automation solutions that require strict mutual exclusivity of two directions combined with overrun time and runtime parameterization.

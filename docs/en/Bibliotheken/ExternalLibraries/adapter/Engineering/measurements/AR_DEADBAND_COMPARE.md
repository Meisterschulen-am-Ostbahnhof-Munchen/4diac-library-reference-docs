# AR_DEADBAND_COMPARE

![AR_DEADBAND_COMPARE_ecc](./AR_DEADBAND_COMPARE_ecc.svg)

* * * * * * * * * *

## Introduction

`AR_DEADBAND_COMPARE` compares a `REAL` measured value `Y` (via an `AR` socket) against a window `[WINDOW_MIN, WINDOW_MAX]` and reports the result via three `AX` plugs: `ABOVE` (`Y > WINDOW_MAX`), `BELOW` (`Y < WINDOW_MIN`) and `CENTERED` (in between). For a symmetric deadband around zero, set `WINDOW_MIN = -deadband` and `WINDOW_MAX = +deadband` (e.g. steering angle centering); the block can equally be used for arbitrary windows (e.g. colour limits).

![AR_DEADBAND_COMPARE](AR_DEADBAND_COMPARE.svg)

## Interface Structure

### **Event Inputs**

No direct event inputs - events arrive via the adapter sockets (`Y.E1`, `WINDOW_MIN.E1`, `WINDOW_MAX.E1`).

### **Event Outputs**

No direct event outputs - events are sent via the adapter plugs (`ABOVE.E1`, `BELOW.E1`, `CENTERED.E1`).

### **Data Inputs**

No direct data inputs (values arrive via the sockets, data port `D1`).

### **Data Outputs**

No direct data outputs (values are output via the plugs, data port `D1`).

### **Adapters**

**Sockets (inputs):**

| Adapter | Type | Description |
| :--- | :--- | :--- |
| `Y` | `adapter::types::unidirectional::AR` | Input measured value to be compared |
| `WINDOW_MIN` | `adapter::types::unidirectional::AR` | Lower window limit (e.g. -deadband or lower threshold) |
| `WINDOW_MAX` | `adapter::types::unidirectional::AR` | Upper window limit (e.g. +deadband or upper threshold) |

**Plugs (outputs):**

| Adapter | Type | Description |
| :--- | :--- | :--- |
| `ABOVE` | `adapter::types::unidirectional::AX` | TRUE if `Y > WINDOW_MAX`; event `E1` only on state change |
| `CENTERED` | `adapter::types::unidirectional::AX` | TRUE if `WINDOW_MIN <= Y <= WINDOW_MAX` (inside window/deadband); event `E1` only on state change |
| `BELOW` | `adapter::types::unidirectional::AX` | TRUE if `Y < WINDOW_MIN`; event `E1` only on state change |

## How it works

The block is a basic FB with an idle state `IDLE` and seven emit states, each of which returns to `IDLE` immediately (condition `1`) after its action.

- **Y change (`Y.E1`)**: The transitions compare the newly computed value of `ABOVE`, `BELOW` and `CENTERED` with the value currently stored in the respective plug (`<>`). Only the outputs whose boolean value actually changes are recomputed and send their `E1`:
    - `EMIT_ABOVE_BELOW`: `ABOVE` and `BELOW` change
    - `EMIT_ABOVE_CENTERED`: `ABOVE` and `CENTERED` change
    - `EMIT_BELOW_CENTERED`: `BELOW` and `CENTERED` change
    - `EMIT_ABOVE_ONLY`, `EMIT_BELOW_ONLY`, `EMIT_CENTERED_ONLY`: only the respective output changes
    - If none of the values changes, `Y.E1` leads directly `IDLE` → `IDLE` with no output event.
- **Window limit change (`WINDOW_MIN.E1` or `WINDOW_MAX.E1`)**: State `EMIT_ALL` recomputes all three outputs and sends `ABOVE.E1`, `BELOW.E1` and `CENTERED.E1` (in practice usually only once at start-up).

Calculations (ST algorithms):

```text
ABOVE.D1    := Y.D1 > WINDOW_MAX.D1;
BELOW.D1    := Y.D1 < WINDOW_MIN.D1;
CENTERED.D1 := (Y.D1 <= WINDOW_MAX.D1) AND (Y.D1 >= WINDOW_MIN.D1);
```

## Technical Details

- **Event efficiency**: On a `Y` change, each plug fires only if its own value actually changes (not all three on every update)
- **Window limits**: On a change of `WINDOW_MIN`/`WINDOW_MAX`, all three outputs are always set again for simplicity
- **Limit values**: The limits themselves belong to `CENTERED` (`<=`/`>=`); `ABOVE` and `BELOW` are strict (`>`/`<`)
- A fixed value for a limit can be supplied via `adapter::types::unidirectional::AR::initval::initval_AR`
- Version history: 1.0 (initial, axle steering centre automation), 1.1 (DEADBAND as AR socket), 2.0 (generalised to an arbitrary window `WINDOW_MIN`/`WINDOW_MAX`)

## State Overview

| State | Description | Action |
| :--- | :--- | :--- |
| **IDLE** | Waits for events on `Y.E1`, `WINDOW_MIN.E1` or `WINDOW_MAX.E1` | - |
| **EMIT_ABOVE_BELOW** | Update ABOVE and BELOW | `ABOVE.E1`, `BELOW.E1` |
| **EMIT_ABOVE_CENTERED** | Update ABOVE and CENTERED | `ABOVE.E1`, `CENTERED.E1` |
| **EMIT_BELOW_CENTERED** | Update BELOW and CENTERED | `BELOW.E1`, `CENTERED.E1` |
| **EMIT_ABOVE_ONLY** | Update ABOVE only | `ABOVE.E1` |
| **EMIT_BELOW_ONLY** | Update BELOW only | `BELOW.E1` |
| **EMIT_CENTERED_ONLY** | Update CENTERED only | `CENTERED.E1` |
| **EMIT_ALL** | Update all three (window limits changed) | `ABOVE.E1`, `BELOW.E1`, `CENTERED.E1` |

## Application Scenarios

- **Steering angle centering**: Checking whether a steering sensor lies within the deadband around 0° (`WINDOW_MIN = -deadband`, `WINDOW_MAX = +deadband`)
- **Window/hysteresis monitoring**: Evaluating limit or colour ranges (e.g. green/red in visualisations)

## Summary

`AR_DEADBAND_COMPARE` provides the three states above, below and inside a window for an `AR` measured value as `AX` signals and generates events only on an actual state change.

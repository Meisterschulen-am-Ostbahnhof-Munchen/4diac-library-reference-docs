# AR_SPLIT_SIGNED

![AR_SPLIT_SIGNED](./AR_SPLIT_SIGNED.svg)

* * * * * * * * * *

## Introduction

The **AR_SPLIT_SIGNED** function block is an adapter wrapper (Composite FB) that splits a signed `REAL` measurement value into two single-sided magnitude components using unidirectional adapters (`adapter::types::unidirectional::AR`):

- `NEG_MAG`: Magnitude of negative deflection (`MAX(0, -Y)`)
- `POS_MAG`: Magnitude of positive deflection (`MAX(0, Y)`)

The block encapsulates calculation FB **SPLIT_SIGNED_REAL** and two **E_D_FF_ANY** D-Flip-Flop blocks to emit the initial event on the first `CLK` and thereafter emit events on plugs `NEG_MAG` and `POS_MAG` **only when the new input value differs from the stored output state** (`D <> Q`).

## Interface Structure

### **Adapter Sockets (Input)**

| Name | Type | Comment |
|---|---|---|
| `Y` | `adapter::types::unidirectional::AR` | Signed input value (AR adapter socket) |

### **Adapter Plugs (Output)**

| Name | Type | Comment |
|---|---|---|
| `NEG_MAG` | `adapter::types::unidirectional::AR` | Negative magnitude (`MAX(0, -Y)`), initial event on 1st call, thereafter on change only |
| `POS_MAG` | `adapter::types::unidirectional::AR` | Positive magnitude (`MAX(0, Y)`), initial event on 1st call, thereafter on change only |

## Functionality

Internally, the composite network consists of three components:

1. **SPLIT (SPLIT_SIGNED_REAL)**: Computes `NEG_MAG` and `POS_MAG` from `Y.D1` upon every `Y.E1` event.
2. **DEDUP_NEG (E_D_FF_ANY)**: An event-driven D-Flip-Flop that emits the initial output event upon the first `CLK` event, and thereafter forwards `CLK` to `EO` and updates `Q := D` only when the new data value `D` differs from the current output state `Q` (`D <> Q`).
3. **DEDUP_POS (E_D_FF_ANY)**: A second D-Flip-Flop of the same type that similarly emits the initial output event on the first call and subsequently emits event `POS_MAG.E1` only when the positive magnitude value changes.

```
Y (AR Adapter Socket)
 ├──> SPLIT (SPLIT_SIGNED_REAL)
       ├──> NEG_MAG ──> DEDUP_NEG (E_D_FF_ANY D-Flip-Flop) ──> NEG_MAG (AR Adapter Plug)
       └──> POS_MAG ──> DEDUP_POS (E_D_FF_ANY D-Flip-Flop) ──> POS_MAG (AR Adapter Plug)
```

## Technical Features

- **Adapter-Driven Architecture:** Fully compatible with unidirectional adapter family `adapter::types::unidirectional::AR`.
- **D-Flip-Flop Event Filtering:** The `E_D_FF_ANY` blocks guarantee by definition that the initial output event is emitted on the first `CLK`, and subsequent output events `EO` are generated only when the new input `D` differs from the stored output `Q`.
- **Overflow Safety:** Inherits saturation logic from `SPLIT_SIGNED_REAL` for two's complement integer limits.

## Application Scenarios

- Connecting bipolar sensors (e.g. steering angle sensor, tilt sensor, joystick) to dual VT bargraph displays.
- Adapter-driven actuation of hydraulic cylinders (extend/retract).

## Comparison with Similar Blocks

- **AR_SPLIT_SIGNED**: Composite FB with adapter interfaces and D-Flip-Flop event filtering.
- **SPLIT_SIGNED_REAL**: Underlying Basic FB for pure signal computation without adapters.

## Conclusion

**AR_SPLIT_SIGNED** delivers an elegant, adapter-driven, and event-efficient solution for signed signal splitting in IEC 61499 applications.

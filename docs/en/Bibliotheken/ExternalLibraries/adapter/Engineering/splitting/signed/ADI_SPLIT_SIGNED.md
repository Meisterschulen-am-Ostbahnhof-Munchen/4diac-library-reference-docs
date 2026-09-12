# ADI_SPLIT_SIGNED

![ADI_SPLIT_SIGNED](./ADI_SPLIT_SIGNED.svg)

* * * * * * * * * *

## Introduction

The **ADI_SPLIT_SIGNED** function block is an adapter wrapper (Composite FB) that splits a signed `DINT` measurement value into two single-sided magnitude components using unidirectional adapters (`adapter::types::unidirectional::ADI`):

- `NEG_MAG`: Magnitude of negative deflection (`MAX(0, -Y)`)
- `POS_MAG`: Magnitude of positive deflection (`MAX(0, Y)`)

The block encapsulates calculation FB **SPLIT_SIGNED_DINT** and two **E_D_FF_ANY** D-Flip-Flop blocks to emit the initial event on the first `CLK` and thereafter emit events on plugs `NEG_MAG` and `POS_MAG` **only when the new input value differs from the stored output state** (`D <> Q`).

## Interface Structure

### **Adapter Sockets (Input)**

| Name | Type | Comment |
|---|---|---|
| `Y` | `adapter::types::unidirectional::ADI` | Signed input value (ADI adapter socket) |

### **Adapter Plugs (Output)**

| Name | Type | Comment |
|---|---|---|
| `NEG_MAG` | `adapter::types::unidirectional::ADI` | Negative magnitude (`MAX(0, -Y)`), initial event on 1st call, thereafter on change only |
| `POS_MAG` | `adapter::types::unidirectional::ADI` | Positive magnitude (`MAX(0, Y)`), initial event on 1st call, thereafter on change only |

## Functionality

Internally, the composite network consists of three components:

1. **SPLIT (SPLIT_SIGNED_DINT)**: Computes `NEG_MAG` and `POS_MAG` from `Y.D1` upon every `Y.E1` event.
2. **DEDUP_NEG (E_D_FF_ANY)**: An event-driven D-Flip-Flop that emits the initial output event upon the first `CLK` event, and thereafter forwards `CLK` to `EO` and updates `Q := D` only when the new data value `D` differs from the current output state `Q` (`D <> Q`).
3. **DEDUP_POS (E_D_FF_ANY)**: A second D-Flip-Flop of the same type that similarly emits the initial output event on the first call and subsequently emits event `POS_MAG.E1` only when the positive magnitude value changes.

```
Y (ADI Adapter Socket)
 ├──> SPLIT (SPLIT_SIGNED_DINT)
       ├──> NEG_MAG ──> DEDUP_NEG (E_D_FF_ANY D-Flip-Flop) ──> NEG_MAG (ADI Adapter Plug)
       └──> POS_MAG ──> DEDUP_POS (E_D_FF_ANY D-Flip-Flop) ──> POS_MAG (ADI Adapter Plug)
```

## Technical Features

- **Adapter-Driven Architecture:** Fully compatible with unidirectional adapter family `adapter::types::unidirectional::ADI`.
- **D-Flip-Flop Event Filtering:** The `E_D_FF_ANY` blocks guarantee by definition that the initial output event is emitted on the first `CLK`, and subsequent output events `EO` are generated only when the new input `D` differs from the stored output `Q`.
- **Overflow Safety:** Inherits saturation logic from `SPLIT_SIGNED_DINT` for two's complement integer limits.

## Application Scenarios

- Connecting bipolar sensors (e.g. steering angle sensor, tilt sensor, joystick) to dual VT bargraph displays.
- Adapter-driven actuation of hydraulic cylinders (extend/retract).

## Comparison with Similar Blocks

- **ADI_SPLIT_SIGNED**: Composite FB with adapter interfaces and D-Flip-Flop event filtering.
- **SPLIT_SIGNED_DINT**: Underlying Basic FB for pure signal computation without adapters.

## Conclusion

**ADI_SPLIT_SIGNED** delivers an elegant, adapter-driven, and event-efficient solution for signed signal splitting in IEC 61499 applications.

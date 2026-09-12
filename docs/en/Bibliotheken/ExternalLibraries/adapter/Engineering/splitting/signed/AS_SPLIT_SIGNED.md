# AS_SPLIT_SIGNED

![AS_SPLIT_SIGNED](./AS_SPLIT_SIGNED.svg)

* * * * * * * * * *

## Introduction

The **AS_SPLIT_SIGNED** function block is an adapter wrapper (Composite FB) that splits a signed `SINT` measurement value into two single-sided magnitude components using unidirectional adapters (`adapter::types::unidirectional::AS`):

- `NEG_MAG`: Magnitude of negative deflection (`MAX(0, -Y)`)
- `POS_MAG`: Magnitude of positive deflection (`MAX(0, Y)`)

The block encapsulates calculation FB **SPLIT_SIGNED_SINT** and two **E_D_FF_ANY** deduplication blocks to emit events on plugs `NEG_MAG` and `POS_MAG` **only when the respective value actually changes**.

## Interface Structure

### **Adapter Sockets (Input)**

| Name | Type | Comment |
|---|---|---|
| `Y` | `adapter::types::unidirectional::AS` | Signed input value (adapter socket) |

### **Adapter Plugs (Output)**

| Name | Type | Comment |
|---|---|---|
| `NEG_MAG` | `adapter::types::unidirectional::AS` | Negative magnitude (`MAX(0, -Y)`), event on change only |
| `POS_MAG` | `adapter::types::unidirectional::AS` | Positive magnitude (`MAX(0, Y)`), event on change only |

## Functionality

Internally, the composite network consists of three components:

1. **SPLIT (SPLIT_SIGNED_SINT)**: Computes `NEG_MAG` and `POS_MAG` from `Y.D1` upon every `Y.E1` event.
2. **DEDUP_NEG (E_D_FF_ANY)**: Compares the new `NEG_MAG` with the stored value. Emits an event on `NEG_MAG.E1` only if the value has changed.
3. **DEDUP_POS (E_D_FF_ANY)**: Compares the new `POS_MAG` with the stored value. Emits an event on `POS_MAG.E1` only if the value has changed.

```
Y (Adapter Socket)
 ├──> SPLIT (SPLIT_SIGNED_SINT)
       ├──> NEG_MAG ──> DEDUP_NEG (E_D_FF_ANY) ──> NEG_MAG (Adapter Plug)
       └──> POS_MAG ──> DEDUP_POS (E_D_FF_ANY) ──> POS_MAG (Adapter Plug)
```

## Technical Features

- **Adapter-Driven Architecture:** Fully compatible with unidirectional adapter family `adapter::types::unidirectional::AS`.
- **Event Efficiency:** Prevents unnecessary event cascades by emitting `NEG_MAG.E1` and `POS_MAG.E1` independently only upon actual value changes.
- **Overflow Safety:** Inherits saturation logic from `SPLIT_SIGNED_SINT` for two's complement integer limits.

## Application Scenarios

- Connecting bipolar sensors (e.g. steering angle sensor, tilt sensor, joystick) to dual VT bargraph displays.
- Adapter-driven actuation of hydraulic cylinders (extend/retract).

## Comparison with Similar Blocks

- **AS_SPLIT_SIGNED**: Composite FB with adapter interfaces and automatic event deduplication.
- **SPLIT_SIGNED_SINT**: Underlying Basic FB for pure signal computation without adapters.

## Conclusion

**AS_SPLIT_SIGNED** delivers an elegant, adapter-driven, and event-efficient solution for signed signal splitting in IEC 61499 applications.

# SPLIT_SIGNED_INT

![SPLIT_SIGNED_INT](./SPLIT_SIGNED_INT.svg)

* * * * * * * * * *

## Introduction

The **SPLIT_SIGNED_INT** function block splits a signed `INT` input value `Y` into two single-sided magnitude components:

- `NEG_MAG = MAX(0, -Y)` (magnitude of negative deflection)
- `POS_MAG = MAX(0, Y)` (magnitude of positive deflection)

It serves as the core calculation block for directional indicators (e.g. separate bargraphs for left/right deflection from center position) or dual directional actuators (e.g. raise/lower or forward/reverse).

## Interface Structure

### **Event Inputs**

| Name | Type | Comment |
|---|---|---|
| `REQ` | `Event` | Request calculation (with `Y`) |

### **Event Outputs**

| Name | Type | Comment |
|---|---|---|
| `CNF` | `Event` | Calculation confirmation (with `NEG_MAG`, `POS_MAG`) |

### **Data Inputs**

| Name | Type | Comment |
|---|---|---|
| `Y` | `INT` | Signed input value |

### **Data Outputs**

| Name | Type | Comment |
|---|---|---|
| `NEG_MAG` | `INT` | `MAX(0, -Y)` – Magnitude of negative deflection |
| `POS_MAG` | `INT` | `MAX(0, Y)` – Magnitude of positive deflection |

## Functionality

Upon receiving event `REQ`, the block executes the following splitting logic:

```pascal
IF Y = INT#-32768 THEN
    NEG_MAG := INT#32767;
ELSE
    NEG_MAG := MAX(INT#0, -Y);
END_IF;
POS_MAG := MAX(INT#0, Y);
```

The block emits confirmation event `CNF` on every `REQ`. Event filtering for unchanged values is handled at the adapter wrapper level (**AI_SPLIT_SIGNED**) using **E_D_FF_ANY** D-Flip-Flop instances.

## Overflow Handling & Saturation

For two's complement integer types, the magnitude of the negative minimum (`-32768`) is 1 greater than the representable positive maximum (`32767`). Mathematical negation of `INT#-32768` without special handling would result in integer overflow and incorrect sign reversal.

**SPLIT_SIGNED_INT** explicitly handles this edge case:

- When `Y = INT#-32768`, `NEG_MAG` saturates to the maximum representable positive value `INT#32767`.
- Control engineering advantage: Saturation at range limit instead of overflow or sign flip.

## Application Scenarios

- Splitting bipolar signals for separate visual components (e.g. left/right bargraphs).
- Driving two unidirectional actuators (e.g. raise vs. lower valves) from a single bipolar setpoint.
- Pre-processing in control loops with direction-dependent deadbands or curves.

## Comparison with Similar Blocks

- **SPLIT_SIGNED_INT**: Pure value/event calculation block for `INT` (Basic FB).
- **AI_SPLIT_SIGNED**: Composite FB wrapper featuring adapter interfaces (`adapter::types::unidirectional::AI`) and automatic per-side D-Flip-Flop event filtering (`E_D_FF_ANY`).

## Conclusion

**SPLIT_SIGNED_INT** provides robust, overflow-safe splitting of `INT` values into positive magnitude components.

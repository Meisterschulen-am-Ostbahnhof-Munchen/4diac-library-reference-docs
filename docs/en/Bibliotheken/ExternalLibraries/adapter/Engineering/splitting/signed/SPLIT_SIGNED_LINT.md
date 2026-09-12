# SPLIT_SIGNED_LINT

![SPLIT_SIGNED_LINT](./SPLIT_SIGNED_LINT.svg)

* * * * * * * * * *

## Introduction

The **SPLIT_SIGNED_LINT** function block splits a signed `LINT` input value `Y` into two single-sided magnitude components:

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
| `Y` | `LINT` | Signed input value |

### **Data Outputs**

| Name | Type | Comment |
|---|---|---|
| `NEG_MAG` | `LINT` | `MAX(0, -Y)` – Magnitude of negative deflection |
| `POS_MAG` | `LINT` | `MAX(0, Y)` – Magnitude of positive deflection |

## Functionality

Upon receiving event `REQ`, the block executes the following splitting logic:

```pascal
IF Y = LINT#-9223372036854775808 THEN
    NEG_MAG := LINT#9223372036854775807;
ELSE
    NEG_MAG := MAX(LINT#0, -Y);
END_IF;
POS_MAG := MAX(LINT#0, Y);
```

The block emits confirmation event `CNF` on every `REQ`. Event filtering for unchanged values is handled at the adapter wrapper level (**ALI_SPLIT_SIGNED**) using **E_D_FF_ANY** D-Flip-Flop instances.

## Overflow Handling & Saturation

For two's complement integer types, the magnitude of the negative minimum (`-9223372036854775808`) is 1 greater than the representable positive maximum (`9223372036854775807`). Mathematical negation of `LINT#-9223372036854775808` without special handling would result in integer overflow and incorrect sign reversal.

**SPLIT_SIGNED_LINT** explicitly handles this edge case:

- When `Y = LINT#-9223372036854775808`, `NEG_MAG` saturates to the maximum representable positive value `LINT#9223372036854775807`.
- Control engineering advantage: Saturation at range limit instead of overflow or sign flip.

## Application Scenarios

- Splitting bipolar signals for separate visual components (e.g. left/right bargraphs).
- Driving two unidirectional actuators (e.g. raise vs. lower valves) from a single bipolar setpoint.
- Pre-processing in control loops with direction-dependent deadbands or curves.

## Comparison with Similar Blocks

- **SPLIT_SIGNED_LINT**: Pure value/event calculation block for `LINT` (Basic FB).
- **ALI_SPLIT_SIGNED**: Composite FB wrapper featuring adapter interfaces (`adapter::types::unidirectional::ALI`) and automatic per-side D-Flip-Flop event filtering (`E_D_FF_ANY`).

## Conclusion

**SPLIT_SIGNED_LINT** provides robust, overflow-safe splitting of `LINT` values into positive magnitude components.

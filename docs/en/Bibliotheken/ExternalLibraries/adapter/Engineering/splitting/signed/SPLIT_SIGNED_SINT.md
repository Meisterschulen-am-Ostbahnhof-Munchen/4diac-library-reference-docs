# SPLIT_SIGNED_SINT

![SPLIT_SIGNED_SINT](./SPLIT_SIGNED_SINT.svg)

* * * * * * * * * *

## Introduction

The **SPLIT_SIGNED_SINT** function block splits a signed `SINT` input value `Y` into two single-sided magnitude components:

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
| `Y` | `SINT` | Signed input value |

### **Data Outputs**

| Name | Type | Comment |
|---|---|---|
| `NEG_MAG` | `SINT` | `MAX(0, -Y)` – Magnitude of negative deflection |
| `POS_MAG` | `SINT` | `MAX(0, Y)` – Magnitude of positive deflection |

## Functionality

Upon receiving event `REQ`, the block executes the following splitting logic:

```pascal
IF Y = SINT#-128 THEN
    NEG_MAG := SINT#127;
ELSE
    NEG_MAG := MAX(SINT#0, -Y);
END_IF;
POS_MAG := MAX(SINT#0, Y);
```

The block emits confirmation event `CNF` on every `REQ`. Event filtering for unchanged values is handled at the adapter wrapper level (**AS_SPLIT_SIGNED**) using **E_D_FF_ANY** D-Flip-Flop instances.

## Overflow Handling & Saturation

For two's complement integer types, the magnitude of the negative minimum (`-128`) is 1 greater than the representable positive maximum (`127`). Mathematical negation of `SINT#-128` without special handling would result in integer overflow and incorrect sign reversal.

**SPLIT_SIGNED_SINT** explicitly handles this edge case:

- When `Y = SINT#-128`, `NEG_MAG` saturates to the maximum representable positive value `SINT#127`.
- Control engineering advantage: Saturation at range limit instead of overflow or sign flip.

## Application Scenarios

- Splitting bipolar signals for separate visual components (e.g. left/right bargraphs).
- Driving two unidirectional actuators (e.g. raise vs. lower valves) from a single bipolar setpoint.
- Pre-processing in control loops with direction-dependent deadbands or curves.

## Comparison with Similar Blocks

- **SPLIT_SIGNED_SINT**: Pure value/event calculation block for `SINT` (Basic FB).
- **AS_SPLIT_SIGNED**: Composite FB wrapper featuring adapter interfaces (`adapter::types::unidirectional::AS`) and automatic per-side D-Flip-Flop event filtering (`E_D_FF_ANY`).

## Conclusion

**SPLIT_SIGNED_SINT** provides robust, overflow-safe splitting of `SINT` values into positive magnitude components.

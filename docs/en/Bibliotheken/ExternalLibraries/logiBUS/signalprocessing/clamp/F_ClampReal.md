# F_ClampReal

![F_ClampReal](./F_ClampReal.svg)

* * * * * * * * * *
## Introduction

The `F_ClampReal` function block (FB) restricts a REAL input value `rIn` to a defined range `[rMin, rMax]`. If `rIn` exceeds `rMax`, the output is set to `rMax` and the over-range flag is activated. If `rIn` is below `rMin`, the output is set to `rMin` and the under-range flag is set. Otherwise, the input passes through unchanged. This FB is useful for safe value limiting in control systems, signal conditioning, and boundary checking.

## Interface Structure

The FB has one event input (`REQ`) and one event output (`CNF`). Data inputs (`rIn`, `rMin`, `rMax`) are read when `REQ` occurs, and the data outputs (`rOut`, `xOver`, `xUnder`) are provided with `CNF`.

### **Event Inputs**

| Event | Description                          |
|-------|--------------------------------------|
| `REQ` | Trigger for performing the clamping operation. |

### **Event Outputs**

| Event | Description                          |
|-------|--------------------------------------|
| `CNF` | Confirmation that the operation has completed and outputs are valid. |

### **Data Inputs**

| Name | Type  | Description                            |
|------|-------|----------------------------------------|
| `rIn` | REAL | Input value to be clamped.             |
| `rMin`| REAL | Lower bound of the valid range.        |
| `rMax`| REAL | Upper bound of the valid range.        |

### **Data Outputs**

| Name   | Type  | Description                                    |
|--------|-------|------------------------------------------------|
| `rOut` | REAL | Clamped result (equal to `rIn` if inside range). This corresponds to the unnamed output in the interface definition. |
| `xOver`| BOOL  | `TRUE` if `rIn` > `rMax`, otherwise `FALSE`.     |
| `xUnder`| BOOL | `TRUE` if `rIn` < `rMin`, otherwise `FALSE`.     |

### **Adapters**

This FB does not use any adapters.

## Functionality

Upon receiving the `REQ` event, the FB evaluates the following logic (written in Structured Text):

- If `rIn > rMax`, then `rOut = rMax`, `xOver = TRUE`, `xUnder = FALSE`.
- Else if `rIn < rMin`, then `rOut = rMin`, `xOver = FALSE`, `xUnder = TRUE`.
- Else, `rOut = rIn`, `xOver = FALSE`, `xUnder = FALSE`.

After the calculation, the `CNF` event is emitted, signalling that the output data are ready. The operation is purely combinatorial and does not rely on internal states.

## Technical Features

- **Pure function**: No internal state, side effects, or memory usage.
- **Data types**: All inputs and the result are `REAL` (floating-point). Flags are `BOOL`.
- **Event‑driven**: Uses the standard `REQ`/`CNF` handshake pattern for synchronous execution.
- **Range validation**: Assumes `rMin ≤ rMax`. If violated, behavior is undefined (but still follows the comparison logic).
- **Implementation**: Written in Structured Text, compatible with IEC 61499 and the 4diac IDE.

## State Overview

The FB is stateless. It does not maintain any information between two invocations. Each execution is independent and based solely on the input values at the moment of `REQ`.

## Application Scenarios

- **Safety interlocks**: Limit a setpoint or measured value to a safe operating range.
- **Signal conditioning**: For sensors that might produce out‑of‑range readings, e.g., clamping a temperature to a plausible band.
- **Control loops**: Prevent integral wind‑up by limiting the controller output.
- **Validation**: Generate flags (`xOver` / `xUnder`) that can be used for alarming or diagnostics.

## Comparison with Similar Blocks

Other clamping blocks may differ in:

- **Return type**: Some FBs only output the clamped value without flags; `F_ClampReal` additionally provides over‑ and under‑range indicators.
- **Input type**: Analogous blocks exist for `INT`, `LREAL`, or other numeric types, but this FB is specifically for `REAL`.
- **Behavior on invalid range**: Some implementations swap `rMin` and `rMax` if `rMax < rMin`; this FB does not perform such a check.

## Conclusion

`F_ClampReal` is a simple, reliable, and reusable function block for clamping REAL values to a specified interval. Its clean interface, stateless design, and integrated flag outputs make it suitable for a wide range of control and monitoring tasks in industrial automation environments.
# CEIL

![CEIL](CEIL.svg)

* * * * * * * * * *

## Introduction

The function `CEIL` is a rounding function (ported from `OSCAT_basic`, `CEIL.TcPOU`) that returns the smallest integer greater than or equal to `X`. The result is clamped to the `INT` range.

## Interface Structure

### **Event Inputs**

- `REQ`: Starts the calculation. Associated data: `X`

### **Event Outputs**

- `CNF`: Signals the calculation and the output of the result.

### **Data Inputs**

- `X` (`REAL`): Value to be rounded

### **Data Outputs**

- (no name) (`INT`): Rounded-up value, also the return value of the function

### **Adapters**

This function uses no adapters.

## How it works

1. If `X >= 32767.0`, the result is `32767`.
2. If `X <= -32768.0`, the result is `-32768`.
3. Otherwise `X` is converted to an integer with `REAL_TO_INT`. If the result is smaller than `X`, 1 is added.

Examples: `CEIL(3.14) = 4`, `CEIL(-3.14) = -3`.

## Technical Details

- Implemented in Structured Text (ST) as a `FUNCTION` without internal state
- Part of the package `OSCAT::Basic::POUs::Mathematical`
- The range check protects against an `INT` overflow (port of 2026-09-17).
- `REAL_TO_INT` is used instead of `TRUNC` (compatibility, revision 1.1 of the OSCAT original).

## Application Scenarios

- Rounding up numeric values, e.g. for display or count values

## Summary

`CEIL` returns the next greater integer of a `REAL` value as `INT` and clamps the result to the `INT` value range.

# ROUND

![ROUND](ROUND.svg)

* * * * * * * * * *

## Introduction

The function `ROUND` rounds a `REAL` value to `N` digits after the decimal point (0 to 8). It was ported from `OSCAT_basic`.

## Interface Structure

### **Event Inputs**

- `REQ`: Starts the calculation. Associated data: `in`, `N`

### **Event Outputs**

- `CNF`: Signals the calculation and the output of the result.

### **Data Inputs**

- `in` (`REAL`): Value to be rounded
- `N` (`INT`): Number of decimal places (limited to 0 to 8)

### **Data Outputs**

- (no name) (`REAL`): Rounded value, also the return value of the function

### **Adapters**

This function uses no adapters.

## How it works

1. The factor `X := EXPT(10.0, LIMIT(0, N, 8))` is determined.
2. If `ABS(in * X) >= 2147483647.0` (DINT range), `in` is returned unchanged.
3. Otherwise the result is `DINT_TO_REAL(REAL_TO_DINT(in * X)) / X`.

## Technical Details

- Implemented in Structured Text (ST) as a `FUNCTION` with local variable `X`, without internal state
- Part of the package `OSCAT::Basic::POUs::Mathematical`
- `EXPT(10.0, LIMIT(0, N, 8))` replaces the global OSCAT constants `decades`; the range check protects against a `DINT` overflow (port of 2026-09-17).
- `REAL_TO_DINT` instead of `TRUNC` fixes the error with negative numbers (OSCAT revision 1.3).
- `N` is limited to 0 to 8.

## Application Scenarios

- Rounding measured values for display or comparison to a fixed number of decimal places

## Summary

`ROUND` rounds a `REAL` value to up to 8 decimal places and returns the input unchanged if the value is too large.

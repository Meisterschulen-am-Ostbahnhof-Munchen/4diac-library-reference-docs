# RND

![RND](RND.svg)

* * * * * * * * * *

## Introduction

The function `RND` rounds a `REAL` number to `N` **significant** digits (not to digits behind the decimal point). It is ported from `OSCAT_basic`. Unlike [`ROUND`](ROUND.md), `N` here denotes the total number of significant digits.

## Interface Structure

### **Event Inputs**

- **REQ**: starts the calculation. Associated data: `X`, `N`.

### **Event Outputs**

- **CNF**: signals the calculation and the output of the result.

### **Data Inputs**

- **X** (`REAL`): value to round
- **N** (`INT`): number of significant digits

### **Data Outputs**

- **(unnamed)** (`REAL`): rounded value, also the return value of the function

### **Adapters**

This function uses no adapters.

## Functionality

1. If `X = 0.0`, the result is `0.0`.
2. Otherwise the factor `M = 10^(LIMIT(1, N, 8) - CEIL(LOG(ABS(X))))` is calculated; the exponent is limited to `-37..8`.
3. If `ABS(X * M) >= 2147483647.0`, `X` is returned unchanged (protection against `DINT` overflow).
4. Otherwise the result is `DINT_TO_REAL(REAL_TO_DINT(X * M)) / M`.

## Technical Details

- Implemented in Structured Text (ST) as a `FUNCTION` without internal state, package `OSCAT::Basic::POUs::Mathematical`
- `N` is limited to `1..8`
- Port of 2026-09-17: limiting of `N` (1..8), of the exponent (-37..8) and a range check protect against `DINT` overflow
- Original version 1.1 (4 Apr 2008, hm): type conversion added to avoid warnings under CODESYS 3.0

## Application Scenarios

- Rounding measured values to a fixed number of significant digits regardless of magnitude

## ⚖️ Comparison with Similar Building Blocks

- [`ROUND`](ROUND.md) rounds to `N` decimal places, `RND` to `N` significant digits.

## Conclusion

`RND` rounds a `REAL` number to `N` significant digits and protects against `DINT` overflow.

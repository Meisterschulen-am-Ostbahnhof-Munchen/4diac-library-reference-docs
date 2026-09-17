# TIME_TO_REAL

![TIME_TO_REAL](TIME_TO_REAL.svg)

* * * * * * * * * *

## Introduction

`TIME_TO_REAL` is a project-specific helper function (`FunctionType`) that converts a time duration of type `TIME` into a floating-point value (`REAL`). The return value represents the duration in **seconds**.

This function is utilized within OSCAT control and filter blocks (such as `FT_PT1`) to format PLC time quantities for mathematical differential and integration formulas.

## Interface Structure

### **Inputs**

| Name | Type | Description |
| :--- | :--- | :----------- |
| `IN` | `TIME` | Input time duration |

### **Outputs**

| Name | Type | Description |
| :--- | :--- | :----------- |
| *(Return)* | `REAL` | Calculated time duration in seconds |

## How It Works

The function converts the internal millisecond representation of the `TIME` input into seconds:

$$\text{Return} = \text{TIME\_TO\_UDINT}(\text{IN}) \cdot 1.0 \times 10^{-3}$$

Examples:

- `IN = T#1s` $\rightarrow$ `Return = 1.0`
- `IN = T#500ms` $\rightarrow$ `Return = 0.5`
- `IN = T#2m30s` $\rightarrow$ `Return = 150.0`

## Technical Features

- **Project-Specific Extension**: `TIME_TO_REAL` is not a standard OSCAT library function, but a local IEC 61499 conversion helper.
- **Scaling Precision**: Uses explicit floating-point scaling ($1.0 \times 10^{-3}$) to prevent integer truncation errors during division.

## See Also

- [`FT_PT1`](FT_PT1.md) – First-order low-pass filter (uses `TIME_TO_REAL`).
- [`FT_PT2`](FT_PT2.md) – Second-order low-pass filter.

# Select_EnableOk_a

![Select_EnableOk_a_network](./Select_EnableOk_a_network.svg)

* * * * * * * * * *

## Introduction

`Select_EnableOk_a` derives one of three values from two `BOOL` values (`bEnable` and `bOk`): `valDisabled` (`bEnable = FALSE`), `valOk` (`bEnable = TRUE`, `bOk = TRUE`) or `valFault` (`bEnable = TRUE`, `bOk = FALSE`). It is a generic, two-stage `F_SEL`; the three values are parameters and not restricted to colors.

![Select_EnableOk_a](Select_EnableOk_a.svg)

## Interface Structure

### **Event Inputs**

- **REQ**: Re-evaluate - `bEnable` and `bOk` are supplied together

### **Event Outputs**

- **CNF**: Value calculated

### **Data Inputs**

- **bEnable** (`BOOL`): Enabled
- **bOk** (`BOOL`): OK state (only relevant if `bEnable = TRUE`)
- **valDisabled** (`USINT`): Output value if `bEnable = FALSE`; set once at instantiation via parameter
- **valOk** (`USINT`): Output value if `bEnable = TRUE` and `bOk = TRUE`; set once at instantiation via parameter
- **valFault** (`USINT`): Output value if `bEnable = TRUE` and `bOk = FALSE`; set once at instantiation via parameter

### **Data Outputs**

- **value** (`USINT`): Selected value

### **Adapters**

This function block uses no adapters.

## Function Blocks (FBs) Used

- **F_SEL_OK_FAULT**: `iec61131::selection::F_SEL` - selects with `G = bOk` between `IN0 = valFault` and `IN1 = valOk`
- **F_MOVE**: `iec61131::selection::F_MOVE` (`DataType=USINT`) - passes the result on
- **F_SEL_ENABLE**: `iec61131::selection::F_SEL` - selects with `G = bEnable` between `IN0 = valDisabled` and `IN1 =` result of `F_MOVE`

## Program Flow and Connections

1. `REQ` → `F_SEL_OK_FAULT.REQ` → `F_MOVE.REQ` → `F_SEL_ENABLE.REQ` → `CNF` (event chain).
2. `bOk` → `F_SEL_OK_FAULT.G`; `valFault` → `IN0`; `valOk` → `IN1`.
3. `F_SEL_OK_FAULT.OUT` → `F_MOVE.IN` → `F_SEL_ENABLE.IN1`.
4. `bEnable` → `F_SEL_ENABLE.G`; `valDisabled` → `F_SEL_ENABLE.IN0`.
5. `F_SEL_ENABLE.OUT` → `value`.

## Technical Details

- The value table:

| bEnable | bOk | value |
| --- | --- | --- |
| FALSE | any | `valDisabled` |
| TRUE | TRUE | `valOk` |
| TRUE | FALSE | `valFault` |

- The three values are `USINT` and can be e.g. color codes, but also any other state values.
- The AX variant of the block is `Select_EnableOk_AX`.

## Application Scenarios

- Three-stage status display (off/ok/fault), e.g. color selection for a VT object

## Summary

`Select_EnableOk_a` selects one of three parameterizable `USINT` values from enable and OK state.

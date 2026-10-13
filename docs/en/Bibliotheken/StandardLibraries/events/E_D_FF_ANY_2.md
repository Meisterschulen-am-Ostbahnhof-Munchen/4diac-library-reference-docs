# E_D_FF_ANY_2

![E_D_FF_ANY_2](E_D_FF_ANY_2.svg)

* * * * * * * * * *

## Introduction

The `E_D_FF_ANY_2` is a data latch (D flip-flop) for **two `ANY` variables** (generic block). The output `EO` is fired only if at least one `Qi` changes due to the clock. It is the extension of `E_D_FF_ANY` to two variables.

## Interface Structure

### **Event Inputs**

- **CLK**: Clock; takes `D1` and `D2` into the latches. Associated data: `D1`, `D2`

### **Event Outputs**

- **EO**: Triggered if the clock results in a change of at least one `Q`. Associated data: `Q1`, `Q2`

### **Data Inputs**

- **D1** (`ANY`): Value 1 to latch
- **D2** (`ANY`): Value 2 to latch

### **Data Outputs**

- **Q1** (`ANY`): Latched value 1
- **Q2** (`ANY`): Latched value 2

### **Adapters**

This function block uses no adapters.

## How it works

On every `CLK` event, `D1` and `D2` are compared with the stored values `Q1` and `Q2`. If at least one `Di` differs from the stored `Qi`, the values are taken over and `EO` is fired. If both values are unchanged, no `EO` is fired.

## Technical Details

- Service interface FB (generic, `ANY` types); it requires the native implementation `GEN_E_D_FF_ANY` (FORTE).
- Unlike the simple D flip-flop `E_D_FF`, `EO` is fired only on an actual change.
- The variant for three variables is `E_D_FF_ANY_3`.

## Application Scenarios

- Jointly latching related values (e.g. control output and control error) and forwarding only on change, to avoid unnecessary follow-up events

## Summary

`E_D_FF_ANY_2` stores two arbitrary values and signals an event only if at least one of them has changed.

# E_D_FF_ANY_1

![E_D_FF_ANY_1](./E_D_FF_ANY_1.svg)

* * * * * * * * * *

## Introduction

`E_D_FF_ANY_1` is the variant of [`E_D_FF_ANY`](E_D_FF_ANY.md) for **one** latched value of any data type (`ANY`) and is part of the family `E_D_FF_ANY_1` to `E_D_FF_ANY_5` with numbered ports (`D1`/`Q1`). It is a clock-driven data latch (D flip-flop) with built-in change detection: the output event `EO` is only triggered if `Q1` changes due to the clock.

## Interface Structure

### **Event Inputs**

- **CLK**: clock, takes over the current value of `D1`
    - **Associated data**: `D1`

### **Event Outputs**

- **EO**: triggered if the clock causes a change of `Q1`
    - **Associated data**: `Q1`

### **Data Inputs**

- **D1** (ANY): value 1 to latch

### **Data Outputs**

- **Q1** (ANY): latched value 1

### **Adapters**

No adapters are used.

## Functionality

On every `CLK` event `D1` is taken over and compared with the previously stored value `Q1`. If `D1` differs from `Q1`, the value is stored and `EO` is triggered; if the value is unchanged, no output event is produced. The block is a generic FB (`eclipse4diac::core::GenericClassName = GEN_E_D_FF_ANY`); the native implementation `GEN_E_D_FF_ANY` (FORTE) is required.

## Technical Details

- **Generic block**: the types of `D1` and `Q1` are `ANY` and are fixed at instantiation
- Port names `D1`/`Q1` (instead of `D`/`Q` for `E_D_FF_ANY`) correspond to the numbered family
- Version 1.0 (2026-08-26): extension of `E_D_FF_ANY` to a numbered variable

## Application Scenarios

- Adapter wrappers around functions with one result, with output only on value change (e.g. `CEIL_AR`)
- Change-driven further processing of one value of any data type

## ⚖️ Comparison with Similar Building Blocks

[`E_D_FF_ANY`](E_D_FF_ANY.md) also stores one value (ports `D`/`Q`), [`E_D_FF_ANY_2`](E_D_FF_ANY_2.md) two, [`E_D_FF_ANY_3`](E_D_FF_ANY_3.md) three, [`E_D_FF_ANY_4`](E_D_FF_ANY_4.md) four and [`E_D_FF_ANY_5`](E_D_FF_ANY_5.md) five values.

## Conclusion

`E_D_FF_ANY_1` is a generic, change-detecting one-value latch that only outputs `EO` if the latched value changes.

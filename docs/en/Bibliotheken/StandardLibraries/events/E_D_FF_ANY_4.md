# E_D_FF_ANY_4

![E_D_FF_ANY_4](./E_D_FF_ANY_4.svg)

* * * * * * * * * *

## Introduction

`E_D_FF_ANY_4` is the variant of [`E_D_FF_ANY`](E_D_FF_ANY.md) for **four** simultaneously latched values of any data type (`ANY`). It is a clock-driven data latch (D flip-flop) with built-in change detection: the output event `EO` is only triggered if at least one of the four outputs `Q1`..`Q4` changes due to the clock.

## Interface Structure

### **Event Inputs**

- **CLK**: clock, takes over the current values of `D1`..`D4`
    - **Associated data**: `D1`, `D2`, `D3`, `D4`

### **Event Outputs**

- **EO**: triggered if the clock causes a change of at least one `Qi`
    - **Associated data**: `Q1`, `Q2`, `Q3`, `Q4`

### **Data Inputs**

- **D1** (ANY): value 1 to latch
- **D2** (ANY): value 2 to latch
- **D3** (ANY): value 3 to latch
- **D4** (ANY): value 4 to latch

### **Data Outputs**

- **Q1** (ANY): latched value 1
- **Q2** (ANY): latched value 2
- **Q3** (ANY): latched value 3
- **Q4** (ANY): latched value 4

### **Adapters**

No adapters are used.

## Functionality

On every `CLK` event `D1`..`D4` are taken over and compared with the previously stored values `Q1`..`Q4`. If at least one `Di` differs from its `Qi`, the values are stored and `EO` is triggered; if all values are unchanged, no output event is produced. The block is a generic FB (`eclipse4diac::core::GenericClassName = GEN_E_D_FF_ANY`).

## Technical Details

- **Generic block**: the types of `D1`..`D4` and `Q1`..`Q4` are `ANY` and are fixed at instantiation
- **Common change detection**: a single `EO` reports the change of any of the four values; which one changed cannot be seen from the event

## Application Scenarios

- Adapter wrappers around functions that deliver four results, with output only on value change
- Change-driven further processing of several values of any data type

## ⚖️ Comparison with Similar Building Blocks

[`E_D_FF_ANY`](E_D_FF_ANY.md) stores one value, [`E_D_FF_ANY_2`](E_D_FF_ANY_2.md) two, [`E_D_FF_ANY_3`](E_D_FF_ANY_3.md) three and `E_D_FF_ANY_4` four values.

## Conclusion

`E_D_FF_ANY_4` is a generic, change-detecting four-value latch that only outputs `EO` if at least one of the four latched values changes.

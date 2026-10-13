# E_D_FF_ANY_3

![E_D_FF_ANY_3](./E_D_FF_ANY_3.svg)

* * * * * * * * * *

## Introduction

The `E_D_FF_ANY_3` is the variant of [`E_D_FF_ANY`](E_D_FF_ANY.md) for **three** simultaneously latched values of arbitrary data type (`ANY`). It is a clock-driven data latch with built-in change detection: the output event `EO` is triggered only if at least one of the three outputs `Q1`..`Q3` actually changes due to the clock.

## Interface Structure

### **Event Inputs**

- **CLK**: clock, takes over the current values of `D1`, `D2` and `D3`
    - **Associated data**: `D1`, `D2`, `D3`

### **Event Outputs**

- **EO**: triggered if the clock results in a change of at least one `Qi`
    - **Associated data**: `Q1`, `Q2`, `Q3`

### **Data Inputs**

- **D1** (ANY): value 1 to latch
- **D2** (ANY): value 2 to latch
- **D3** (ANY): value 3 to latch

### **Data Outputs**

- **Q1** (ANY): latched value 1
- **Q2** (ANY): latched value 2
- **Q3** (ANY): latched value 3

### **Adapters**

No adapters are used.

## Functionality

On every `CLK` event, `D1`, `D2` and `D3` are taken over and compared with the previously stored values `Q1`, `Q2` and `Q3`. If at least one `Di` differs from its `Qi`, the values are stored and `EO` is triggered; if all values are unchanged, no output event is produced. The block is a generic service interface FB whose native implementation is `GEN_E_D_FF_ANY` (FORTE).

## Technical Details

- **Generic block**: the types of `D1`..`D3` and `Q1`..`Q3` are `ANY` and are fixed on instantiation
- **Common change detection**: a single `EO` reports the change of any of the three values; which one has changed cannot be seen from the event
- Serves to pass on several related values (e.g. control output, control error and limit flag of a controller) with just one event and to avoid follow-up events when the values are unchanged

## Application Scenarios

- Adapter wrappers around functions delivering three results, with output only on value change (e.g. `CTRL_PI_AR`)
- Change-driven further processing of several values of arbitrary type

## ⚖️ Comparison with Similar Blocks

`E_D_FF_ANY` latches one value, `E_D_FF_ANY_3` three. Corresponding variants with another number of values exist as `E_D_FF_ANY_1`, `E_D_FF_ANY_2`, `E_D_FF_ANY_4` and `E_D_FF_ANY_5`.

## Conclusion

`E_D_FF_ANY_3` is a generic, change-detecting three-value latch that outputs `EO` only if at least one of the three latched values changes.

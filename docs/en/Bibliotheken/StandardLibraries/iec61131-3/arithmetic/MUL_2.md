# MUL_2

![MUL_2](./MUL_2.svg)

* * * * * * * * * *

## Introduction

The **MUL_2** is a generic function block that calculates the arithmetical product of two numeric inputs (`OUT = IN1 * IN2`). The data types of the ports (`ANY_NUM`) are fixed on instantiation; the native implementation is `GEN_MUL`.

## Interface Structure

### **Event Inputs**

- **REQ**: normal execution request (with `IN1`, `IN2`)

### **Event Outputs**

- **CNF**: execution confirmation (with `OUT`)

### **Data Inputs**

- **IN1** (ANY_NUM): first factor
- **IN2** (ANY_NUM): second factor

### **Data Outputs**

- **OUT** (ANY_NUM): product

### **Adapters**

No adapters are used.

## Functionality

On `REQ`, `IN1` and `IN2` are read, multiplied, and the product is output at `OUT` together with the event `CNF`.

## Technical Details

- Generic block (generic class `GEN_MUL`): the port types are `ANY_NUM` and are fixed per instance
- Stateless service interface block

## Application Scenarios

- Scaling a value by a factor in an event network
- Area, power or quantity calculations from two quantities

## ⚖️ Comparison with Similar Blocks

[`ADD_2`](ADD_2.md) correspondingly forms the sum of two values; for multiplication with adapters, there are the AR-based variants in the adapter library.

## Conclusion

`MUL_2` multiplies two numeric values of arbitrary `ANY_NUM` type in an event-driven way.

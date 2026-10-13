# MUL_3

![MUL_3](./MUL_3.svg)

* * * * * * * * * *

## Introduction

**MUL_3** is a generic function block that calculates the arithmetic product of three numeric inputs (`OUT = IN1 * IN2 * IN3`). The data types of the ports (`ANY_NUM`) are fixed at instantiation; the native implementation is `GEN_MUL`.

## Interface Structure

### **Event Inputs**

- **REQ**: normal execution request (with `IN1`, `IN2`, `IN3`)

### **Event Outputs**

- **CNF**: execution confirmation (with `OUT`)

### **Data Inputs**

- **IN1** (ANY_NUM): first factor
- **IN2** (ANY_NUM): second factor
- **IN3** (ANY_NUM): third factor

### **Data Outputs**

- **OUT** (ANY_NUM): product

### **Adapters**

No adapters are used.

## Functionality

On `REQ`, `IN1`, `IN2` and `IN3` are read, multiplied, and the product is output at `OUT` together with the event `CNF`.

## Technical Details

- Generic block (generic class `GEN_MUL`): the port types are `ANY_NUM` and are fixed per instance
- Stateless service interface block

## Application Scenarios

- Product of three quantities in an event network (e.g. volume calculation from length, width, height)

## ⚖️ Comparison with Similar Building Blocks

[`MUL_2`](MUL_2.md) multiplies two values, `MUL_3` three.

## Conclusion

`MUL_3` multiplies three numeric values of any `ANY_NUM` type in an event-driven manner.

# F_NumericObjectPool_S_TO_OBJID

![F_NumericObjectPool_S_TO_OBJID](./F_NumericObjectPool_S_TO_OBJID.svg)

* * * * * * * * * *

## Introduction

`F_NumericObjectPool_S_TO_OBJID` extracts the ISOBUS object ID (`u16ObjId`) from a `NumericObjectPool_S` structure (`logiBUS::utils::conversion::phys::NumericObjectPool_S`).

## Interface Structure

### **Event Inputs**

- **REQ**: execution request (associated with `stObj`)

### **Event Outputs**

- **CNF**: confirmation (associated with the function result)

### **Data Inputs**

- **stObj** (`logiBUS::utils::conversion::phys::NumericObjectPool_S`): object pool structure of a VT number variable

### **Data Outputs**

- **(function result)** (UINT): the object ID `stObj.u16ObjId`

### **Adapters**

No adapters are used.

## Functionality

The function consists of a single assignment: `F_NumericObjectPool_S_TO_OBJID := stObj.u16ObjId;`. On `REQ`, the object ID is read from the structure and output with `CNF`.

## Technical Details

- Pure function without state, version 1.0 (2026-10-11)
- The output has no name of its own but is the return value of the function
- The function is located in the package `logiBUS::utils::conversion::phys`

## Application Scenarios

- Access to the object ID of a VT number variable when a block expects only the plain `UINT` ID instead of the whole `NumericObjectPool_S` structure

## Conclusion

`F_NumericObjectPool_S_TO_OBJID` returns the `u16ObjId` of a `NumericObjectPool_S` structure as a `UINT`.

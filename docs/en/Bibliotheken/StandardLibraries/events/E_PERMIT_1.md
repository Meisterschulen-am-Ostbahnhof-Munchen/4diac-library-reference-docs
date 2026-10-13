# E_PERMIT_1

![E_PERMIT_1](E_PERMIT_1.svg)

* * * * * * * * * *

## Introduction

The `E_PERMIT_1` is a function block for the **permissive propagation of one event channel** (generic block). The event `EI1` is forwarded to `EO1` only if the permit condition `PERMIT` is fulfilled. It is the one-channel variant of the generic `E_PERMIT` family.

## Interface Structure

### **Event Inputs**

- **EI1**: Event input, channel 1. Associated data: `PERMIT`

### **Event Outputs**

- **EO1**: Event output, channel 1

### **Data Inputs**

- **PERMIT** (`BOOL`): Permit condition

### **Data Outputs**

*No data outputs*

### **Adapters**

This function block uses no adapters.

## How it works

On every `EI1` event, `PERMIT` is evaluated. If `PERMIT = TRUE`, `EO1` is fired; otherwise the event is suppressed.

## Technical Details

- Generic block that requires the native implementation `GEN_E_PERMIT` (FORTE).
- Pure event filter: no data is stored or output.
- `PERMIT` is associated with `EI1` and is therefore read anew on every event.
- See also [`E_PERMIT`](E_PERMIT.md) (standard variant).

## Application Scenarios

- Forwarding events only while a permit is active, e.g. key events only in automatic mode
- Blocking event paths by a status signal

## Summary

`E_PERMIT_1` forwards a single event only if the `PERMIT` condition is fulfilled.

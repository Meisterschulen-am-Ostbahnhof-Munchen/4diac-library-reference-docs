# INIT_ON_OFF

![INIT_ON_OFF_network](./INIT_ON_OFF_network.svg)

* * * * * * * * * *

## Introduction

`INIT_ON_OFF` is the counterpart of `E_TRIG`, extended by two independent pass-throughs: `INIT_ON`/`INITO_ON` (event type `EInitOn`) and `INIT_OFF`/`INITO_OFF` (event type `EInitOff`). The block contains no logic; every input event is passed directly to the associated output.

![INIT_ON_OFF](INIT_ON_OFF.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): Initialization request
- **INIT_ON** (`EInitOn`): Initialization request, own channel independent of `INIT`
- **INIT_OFF** (`EInitOff`): Initialization request, own channel independent of `INIT`
- **REQ**: Service request

### **Event Outputs**

- **INITO** (`EInit`): Initialization confirmation
- **INITO_ON** (`EInitOn`): Initialization confirmation, own channel independent of `INITO`
- **INITO_OFF** (`EInitOff`): Initialization confirmation, own channel independent of `INITO`
- **CNF**: Confirmation of the requested service

### **Data Inputs**

*No data inputs*

### **Data Outputs**

*No data outputs*

### **Adapters**

This function block uses no adapters.

## How it works

Pure event connections:

- `INIT` → `INITO`
- `REQ` → `CNF`
- `INIT_ON` → `INITO_ON`
- `INIT_OFF` → `INITO_OFF`

## Technical Details

- Composite function block (`FBNetwork`) without data and without internal state
- The three initialization channels are independent of each other, so that on- and off-related initializations can be wired separately.
- See also [`INIT`](INIT.md).

## Application Scenarios

- Separate initialization events for the on and off path in a network

## 🛠️ Related Exercises

- [Uebung_006g_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006g_AX/)

## Summary

`INIT_ON_OFF` provides three independent initialization pass-throughs and one `REQ`/`CNF` pass-through.

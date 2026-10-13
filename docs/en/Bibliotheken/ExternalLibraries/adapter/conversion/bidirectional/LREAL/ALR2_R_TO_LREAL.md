# ALR2_R_TO_LREAL

![ALR2_R_TO_LREAL_network](./ALR2_R_TO_LREAL_network.svg)

* * * * * * * * * *

## Introduction

`ALR2_R_TO_LREAL` is a composite FB that mediates between a `LREAL` data pair (`OUT`/`IN`) and the bidirectional adapter `ALR2`. In contrast to the plug variant `ALR2_LREAL_TO_R` it has an adapter **socket** (input). It completes the conversion family analogously to `AR2`/`AB2`/`AE2`.

![ALR2_R_TO_LREAL](ALR2_R_TO_LREAL.svg)

## Interface Structure

### **Event Inputs**

- `REQ`: Request to the adapter. Associated data: `OUT`.

### **Event Outputs**

- `CNF`: Confirmation from the adapter. Associated data: `IN`.

### **Data Inputs**

- `OUT` (`LREAL`): Value sent to the adapter.

### **Data Outputs**

- `IN` (`LREAL`): Value received from the adapter.

### **Adapters**

| Direction | Name | Adapter type | Comment |
| :--- | :--- | :--- | :--- |
| **Socket** (input) | `ALR2_IN` | `adapter::types::bidirectional::ALR2` | Adapter input. |

## Functionality

The block contains no internal FBs, only connections:

- `REQ` → `ALR2_IN.EI1` and `OUT` → `ALR2_IN.DI1`: a `REQ` event sends the value `OUT` through the adapter.
- `ALR2_IN.EO1` → `CNF` and `ALR2_IN.DO1` → `IN`: an event coming from the adapter is output as `CNF`, and the received value appears at `IN`.

## Technical Details

- Pure pass-through without logic and without internal state.
- The data type is `LREAL` (64-bit floating point).
- The naming `OUT`/`IN` is chosen from the adapter's point of view: `OUT` goes out to the adapter, `IN` comes in from the adapter.

## Application Scenarios

- Output a `LREAL` value to a bidirectional `ALR2` adapter and at the same time read its backward channel as a data output, when the adapter is present as a socket.

## Summary

`ALR2_R_TO_LREAL` connects events and `LREAL` data 1:1 with the bidirectional adapter `ALR2` (socket variant).

# AUI2_I_TO_UINT

![AUI2_I_TO_UINT_network](./AUI2_I_TO_UINT_network.svg)

* * * * * * * * * *

## Introduction

`AUI2_I_TO_UINT` is a composite FB that mediates between a `UINT` data pair (`OUT`/`IN`) and the bidirectional adapter `AUI2`. In contrast to the plug variant `AUI2_UINT_TO_R` it has an adapter **socket** (input). It completes the conversion family analogously to `AR2`/`AB2`/`AE2`.

![AUI2_I_TO_UINT](AUI2_I_TO_UINT.svg)

## Interface Structure

### **Event Inputs**

- `REQ`: Request to the adapter. Associated data: `OUT`.

### **Event Outputs**

- `CNF`: Confirmation from the adapter. Associated data: `IN`.

### **Data Inputs**

- `OUT` (`UINT`): Value sent to the adapter.

### **Data Outputs**

- `IN` (`UINT`): Value received from the adapter.

### **Adapters**

| Direction | Name | Adapter type | Comment |
| :--- | :--- | :--- | :--- |
| **Socket** (input) | `AUI2_IN` | `adapter::types::bidirectional::AUI2` | Adapter input. |

## Functionality

The block contains no internal FBs, only connections:

- `REQ` → `AUI2_IN.EI1` and `OUT` → `AUI2_IN.DI1`: a `REQ` event sends the value `OUT` through the adapter.
- `AUI2_IN.EO1` → `CNF` and `AUI2_IN.DO1` → `IN`: an event coming from the adapter is output as `CNF`, and the received value appears at `IN`.

## Technical Details

- Pure pass-through without logic and without internal state.
- The data type is `UINT` (unsigned 16-bit integer).
- The naming `OUT`/`IN` is chosen from the adapter's point of view: `OUT` goes out to the adapter, `IN` comes in from the adapter.

## Application Scenarios

- Output a `UINT` value to a bidirectional `AUI2` adapter and at the same time read its backward channel as a data output, when the adapter is present as a socket.

## Summary

`AUI2_I_TO_UINT` connects events and `UINT` data 1:1 with the bidirectional adapter `AUI2` (socket variant).

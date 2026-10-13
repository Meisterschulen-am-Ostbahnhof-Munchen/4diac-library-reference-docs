# ALR2_LREAL_TO_R

![ALR2_LREAL_TO_R_network](./ALR2_LREAL_TO_R_network.svg)

* * * * * * * * * *

## Introduction

`ALR2_LREAL_TO_R` is a composite FB that mediates between an `LREAL` data pair (`OUT`/`IN`) and the bidirectional adapter `ALR2`. It completes the conversion family analogously to `AR2`/`AB2`/`AE2`.

![ALR2_LREAL_TO_R](ALR2_LREAL_TO_R.svg)

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
| **Plug** (output) | `ALR2_OUT` | `adapter::types::bidirectional::ALR2` | Adapter output. |

## Functionality

The block contains no internal FBs, only connections:

- `REQ` → `ALR2_OUT.EO1` and `OUT` → `ALR2_OUT.DO1`: a `REQ` event sends the value `OUT` through the adapter.
- `ALR2_OUT.EI1` → `CNF` and `ALR2_OUT.DI1` → `IN`: an event received through the adapter is output as `CNF`, and the received value appears at `IN`.

## Technical Details

- Pure pass-through without logic.
- The data type is `LREAL` (64-bit floating point).
- The naming `OUT`/`IN` is chosen from the adapter's point of view: `OUT` goes out to the adapter, `IN` comes in from the adapter.

## Application Scenarios

- Output an `LREAL` value to a bidirectional `ALR2` adapter and at the same time read its backward channel as a data output.

## Summary

`ALR2_LREAL_TO_R` connects events and `LREAL` data 1:1 with the bidirectional adapter `ALR2`.

---

### 🌐 Related topic pages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

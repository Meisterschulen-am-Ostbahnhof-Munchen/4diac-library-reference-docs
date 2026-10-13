# SingleSelect

![SingleSelect_network](./SingleSelect_network.svg)

* * * * * * * * * *

## Introduction

`SingleSelect` is a single selection (radio button behaviour) with five outputs: a selection index at the AUI socket `IN` activates exactly one of the five AX outputs `OUT1`..`OUT5`; the others are reset in the process. The interlocking is performed by a chain of five `ILOCK_T_FF_SR_AX`.

![SingleSelect](SingleSelect.svg)

## Interface Structure

### **Event Inputs**

The function block has no direct event inputs.

### **Event Outputs**

The function block has no direct event outputs.

### **Data Inputs**

The function block has no direct data inputs.

### **Data Outputs**

The function block has no direct data outputs.

### **Adapters**

- **IN** (socket, `adapter::types::unidirectional::AUI`): selection index (UINT)
- **OUT1**..**OUT5** (plugs, `adapter::types::unidirectional::AX`): state of the five selection positions

## Functionality

- `IN` → `AUI_DEMUX.K`: `AUI_DEMUX` ([`adapter::events::unidirectional::AUI_DEMUX_5`](../../adapter/events/unidirectional/UINT/AUI_DEMUX_5.md)) forwards the event depending on the index: `K = 0` → `EO1`, `K = 1` → `EO2`, `K = 2` → `EO3`, `K = 3` → `EO4`, `K = 4` → `EO5`.
- `AUI_DEMUX.EOn` → `ILOCK_T_FF_SR_AX_n.S` (n = 1..5): the selected flip-flop is set.
- `ILOCK_T_FF_SR_AX_n.Q` → `OUTn`: the state of each flip-flop appears at the associated output.
- `ILOCK_T_FF_SR_AX_n.ILOCK_OUT` → `ILOCK_T_FF_SR_AX_(n+1).ILOCK_IN` (n = 1..4): the five flip-flops are connected into a chain via their bidirectional `AE2` interlock adapters. When one element sets its output, it reports this through the chain and the other elements are reset (see [`ILOCK_T_FF_SR_AX`](../../logiBUS/signalprocessing/interlock/ILOCK_T_FF_SR_AX.md)).

## Technical Details

- The block has no event or data ports; everything runs via adapters
- Version 1.1 tidied up the layout: `AUI_DEMUX_4` was renamed to `AUI_DEMUX` (the `_4` had no meaning)
- Valid indices are 0 to 4; the mapping between index and output is `K = n - 1` ↔ `OUTn`

## Application Scenarios

- Selection menus or operating mode selection in which only one of five options may be active at a time (e.g. via a VT selection list)

## Conclusion

`SingleSelect` converts a selection index into a single selection with five AX outputs using a demultiplexer and an interlock chain of five `ILOCK_T_FF_SR_AX`.

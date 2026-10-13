# Aux_IE_1

![Aux_IE_1_network](./Aux_IE_1_network.svg)

* * * * * * * * * *

## Introduction

`Aux_IE_1` reads an AuxFunction2 object and outputs a configurable event (`AuxiliaryState`, e.g. `AuxEnabled`) as a pure `IND` event. The block is a thin wrapper around `isobus::UT::io::Auxiliary::IN::Aux_IE`, analogous to [`Softkey_IE_1`](Softkey_IE_1.md) and [`Button_IE_1`](Button_IE_1.md). Convention in this project: the isobus IE blocks are never used raw inline but always wrapped.

![Aux_IE_1](Aux_IE_1.svg)

## Interface Structure

### **Event Inputs**

The function block has no direct event inputs.

### **Event Outputs**

- **IND**: fires when the configured `InputEvent` occurs

### **Data Inputs**

- **u16ObjId** (UINT): object ID of the AuxFunction2 (default `ID_NULL`)
- **InputEvent** (`isobus::UT::io::Auxiliary::IN::AuxiliaryState_S`): e.g. `AuxEnabled`, `AuxDisabled` (default `Invalid`)

### **Data Outputs**

The function block has no direct data outputs.

### **Adapters**

No adapters are used.

## Functionality

The network contains exactly one instance `Aux` of type `isobus::UT::io::Auxiliary::IN::Aux_IE` with the parameter `QI = TRUE`. `u16ObjId` and `InputEvent` are routed unchanged to the like-named inputs of the instance, and its event `IND` is output directly as the `IND` of the SubApp.

## Technical Details

- Pure pass-through without logic or state of its own
- `QI` is hard-wired to TRUE; the block is therefore always active
- Because `InputEvent` is a parameter, the same SubApp can be used for different kinds of events (e.g. pressing or releasing)

## Application Scenarios

- Reading a single VT operation as an event without using the raw IE block directly in the network

## ⚖️ Comparison with Similar Blocks

Equivalent wrappers are [`Button_IE_1`](Button_IE_1.md) and [`Softkey_IE_1`](Softkey_IE_1.md); they differ only in the VT object type and in the enumeration type of `InputEvent`.

## Conclusion

`Aux_IE_1` is a minimal wrapper that provides a configurable AuxFunction2 event as `IND`.

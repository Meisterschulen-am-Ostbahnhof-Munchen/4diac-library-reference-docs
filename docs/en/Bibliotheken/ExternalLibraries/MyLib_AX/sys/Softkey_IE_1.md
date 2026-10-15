# Softkey_IE_1

![Softkey_IE_1_network](./Softkey_IE_1_network.svg)

* * * * * * * * * *

## Introduction

`Softkey_IE_1` reads a SoftKey object and outputs a configurable event (`SoftKeyActivationCode`, e.g. `SK_PRESSED`/`SK_RELEASED`) as a pure `IND` event. The block is a thin wrapper around `isobus::UT::io::Softkey::Softkey_IE`, analogous to [`Aux_IE_1`](Aux_IE_1.md) and [`Button_IE_1`](Button_IE_1.md). Convention in this project: `Softkey_IE` is never used raw inline but always wrapped.

![Softkey_IE_1](Softkey_IE_1.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request

### **Event Outputs**

- **INITO** (`EInit`): initialization confirm
- **IND**: fires when the configured `InputEvent` occurs

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): quality input, TRUE enables the service
- **u16ObjId** (UINT): object ID of the SoftKey (default `ID_NULL`)
- **InputEvent** (`isobus::UT::io::Softkey::SoftKeyActivationCode_S`): e.g. `SK_PRESSED`, `SK_RELEASED` (default `Invalid`)

### **Data Outputs**

- **QO** (`BOOL`): quality output, taken directly from `SoftKey.QO`

### **Adapters**

No adapters are used.

## Functionality

The network contains exactly one instance `SoftKey` of type `isobus::UT::io::Softkey::Softkey_IE` (`QI` comes from the subapp input `QI`). `u16ObjId` and `InputEvent` are routed unchanged to the like-named inputs of the instance, and its event `IND` is output directly as the `IND` of the SubApp.

## Technical Details

- Pure pass-through without logic or state of its own
- `QI` is exposed as an input (default `TRUE`) and `QO` comes directly from `SoftKey.QO`; left unwired, the block is therefore active
- Because `InputEvent` is a parameter, the same SubApp can be used for different kinds of events (e.g. pressing or releasing)

## Application Scenarios

- Reading a single VT operation as an event without using the raw IE block directly in the network

## ⚖️ Comparison with Similar Blocks

Equivalent wrappers are [`Aux_IE_1`](Aux_IE_1.md) and [`Button_IE_1`](Button_IE_1.md); they differ only in the VT object type and in the enumeration type of `InputEvent`.

## Conclusion

`Softkey_IE_1` is a minimal wrapper that provides a configurable SoftKey event as `IND`.

# Button_IE_1

![Button_IE_1_network](./Button_IE_1_network.svg)

* * * * * * * * * *

## Introduction

`Button_IE_1` reads a button object and outputs a configurable event (`ButtonActivationCode`, e.g. `BT_PRESSED_LATCHED`) as a pure `IND` event. The block is a thin wrapper around `isobus::UT::io::Button::Button_IE`, analogous to [`Aux_IE_1`](Aux_IE_1.md) and [`Softkey_IE_1`](Softkey_IE_1.md). Convention in this project: the isobus IE blocks are never used raw inline but always wrapped.

![Button_IE_1](Button_IE_1.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): Initialization Request

### **Event Outputs**

- **INITO** (`EInit`): Initialization Confirm
- **IND**: fires when the configured `InputEvent` occurs

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): Quality Input - TRUE to enable the service
- **u16ObjId** (UINT): object ID of the button (default `ID_NULL`)
- **InputEvent** (`isobus::UT::io::Button::ButtonActivationCode_S`): e.g. `BT_PRESSED_LATCHED`, `BT_RELEASED_UNLATCHED` (default `Invalid`)

### **Data Outputs**

- **QO** (`BOOL`): Quality Output - TRUE if the service executed successfully (passed through from `Button.QO`)

### **Adapters**

No adapters are used.

## Functionality

The network contains exactly one instance `Button` of type `isobus::UT::io::Button::Button_IE`. `u16ObjId`, `InputEvent` and `QI` are routed unchanged to the like-named inputs of the instance, `INIT` → `Button.INIT`; its events `IND` and `INITO` and its output `QO` are output directly as `IND`, `INITO` and `QO` of the SubApp.

## Technical Details

- Pure pass-through without logic or state of its own
- `QI` is passed through from outside to the inner block (default `TRUE`)
- Because `InputEvent` is a parameter, the same SubApp can be used for different kinds of events (e.g. pressing or releasing)

## Application Scenarios

- Reading a single VT operation as an event without using the raw IE block directly in the network

## ⚖️ Comparison with Similar Blocks

Equivalent wrappers are [`Aux_IE_1`](Aux_IE_1.md) and [`Softkey_IE_1`](Softkey_IE_1.md); they differ only in the VT object type and in the enumeration type of `InputEvent`.

## Conclusion

`Button_IE_1` is a minimal wrapper that provides a configurable button event as `IND`.

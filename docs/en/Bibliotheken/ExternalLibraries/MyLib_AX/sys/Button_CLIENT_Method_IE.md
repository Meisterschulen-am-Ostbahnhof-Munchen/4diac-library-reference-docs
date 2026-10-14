# Button_CLIENT_Method_IE

![Button_CLIENT_Method_IE_network](./Button_CLIENT_Method_IE_network.svg)

* * * * * * * * * *

## Introduction

`Button_CLIENT_Method_IE` uses a VT button (`Button_IE`) to trigger, via `CLIENT_0`, a remote **OPC-UA method call** without arguments and return value (`ACTION=CALL_METHOD`). It is the general pattern "VT key calls a method on another station" and was extracted from `logiBUS_AI_Calibrated_Subscribe_VT_OPC`, where it occurred twice identically. Unlike `Button_Method_IE` (button OR local method → common `IND`), this is the reverse "caller" case: button → remote method.

![Button_CLIENT_Method_IE](Button_CLIENT_Method_IE.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request

### **Event Outputs**

- **INITO** (`EInit`): initialization confirm

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): quality input, TRUE enables the service
- **u16ObjId** (`UINT`): Object ID of the button
- **InputEvent** (`isobus::UT::io::Button::ButtonActivationCode_S`): Which button activation triggers the method call (e.g. `BT_PRESSED_LATCHED`, `BT_RELEASED_UNLATCHED`)
- **ID_CALL** (`WSTRING`): Remote method address (`ACTION=CALL_METHOD`); calls the corresponding `ID_..._METHOD` address (`ACTION=CREATE_METHOD`) on the target station

### **Data Outputs**

- **QO** (`BOOL`): quality output, AND combination (`AND_QO`) of the `QO` outputs of `Button` and `CLIENT`

### **Adapters**

This function block uses no adapters.

## Function Blocks (FBs) Used

- **Button**: [`isobus::UT::io::Button::Button_IE`](../../isobus/UT/io/Button/Button_IE.md) (`QI` from the wrapper input)
- **CLIENT**: [`iec61499::net::CLIENT_0`](../../../StandardLibraries/net/CLIENT_0.md) (`QI` from the wrapper input)
- **AND_QO**: `iec61131::booleanOperators::AND_BOOL_2` - combines `Button.QO` and `CLIENT.QO` into `QO`

## Program Flow and Connections

- `u16ObjId` → `Button.u16ObjId`; `InputEvent` → `Button.InputEvent`
- `Button.IND` → `CLIENT.REQ`
- `ID_CALL` → `CLIENT.ID`
- Initialization: `INIT` → `Button.INIT`; `Button.INITO` → `CLIENT.INIT`; `CLIENT.INITO` → `INITO` and → `AND_QO.REQ`
- Quality: `QI` → `Button.QI` and `CLIENT.QI`; `Button.QO` → `AND_QO.IN1`; `CLIENT.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`

On every button activation matching `InputEvent`, the method call `CLIENT_0` is thus triggered.

## Technical Details

- `InputEvent` is a parameter and not hard-coded to `BT_PRESSED_LATCHED`, so that other activation types (e.g. release) are reusable as well.
- The confirmation (`CNF`) of `CLIENT_0` is not exposed.

## Application Scenarios

- VT key that calls a method on another station (e.g. calibration via OPC-UA)

## Summary

`Button_CLIENT_Method_IE` is a small, generic pattern for calling a remote OPC-UA method via a VT key.

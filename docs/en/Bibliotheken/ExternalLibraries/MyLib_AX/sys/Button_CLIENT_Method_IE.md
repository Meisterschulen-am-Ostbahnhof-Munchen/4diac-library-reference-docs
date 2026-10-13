# Button_CLIENT_Method_IE

![Button_CLIENT_Method_IE_network](./Button_CLIENT_Method_IE_network.svg)

* * * * * * * * * *

## Introduction

`Button_CLIENT_Method_IE` uses a VT button (`Button_IE`) to trigger, via `CLIENT_0`, a remote **OPC-UA method call** without arguments and return value (`ACTION=CALL_METHOD`). It is the general pattern "VT key calls a method on another station" and was extracted from `logiBUS_AI_Calibrated_Subscribe_VT_OPC`, where it occurred twice identically. Unlike `Button_Method_IE` (button OR local method → common `IND`), this is the reverse "caller" case: button → remote method.

![Button_CLIENT_Method_IE](Button_CLIENT_Method_IE.svg)

## Interface Structure

### **Event Inputs**

*No direct event inputs*

### **Event Outputs**

*No direct event outputs*

### **Data Inputs**

- **u16ObjId** (`UINT`): Object ID of the button
- **InputEvent** (`isobus::UT::io::Button::ButtonActivationCode_S`): Which button activation triggers the method call (e.g. `BT_PRESSED_LATCHED`, `BT_RELEASED_UNLATCHED`)
- **ID_CALL** (`WSTRING`): Remote method address (`ACTION=CALL_METHOD`); calls the corresponding `ID_..._METHOD` address (`ACTION=CREATE_METHOD`) on the target station

### **Data Outputs**

*No data outputs*

### **Adapters**

This function block uses no adapters.

## Function Blocks (FBs) Used

- **Button**: [`isobus::UT::io::Button::Button_IE`](../../isobus/UT/io/Button/Button_IE.md) (`QI=TRUE`)
- **CLIENT**: [`iec61499::net::CLIENT_0`](../../../StandardLibraries/net/CLIENT_0.md) (`QI=TRUE`)

## Program Flow and Connections

- `u16ObjId` → `Button.u16ObjId`; `InputEvent` → `Button.InputEvent`
- `Button.IND` → `CLIENT.REQ`
- `ID_CALL` → `CLIENT.ID`

On every button activation matching `InputEvent`, the method call `CLIENT_0` is thus triggered.

## Technical Details

- `InputEvent` is a parameter and not hard-coded to `BT_PRESSED_LATCHED`, so that other activation types (e.g. release) are reusable as well.
- The confirmation (`CNF`) of `CLIENT_0` is not exposed.

## Application Scenarios

- VT key that calls a method on another station (e.g. calibration via OPC-UA)

## Summary

`Button_CLIENT_Method_IE` is a small, generic pattern for calling a remote OPC-UA method via a VT key.

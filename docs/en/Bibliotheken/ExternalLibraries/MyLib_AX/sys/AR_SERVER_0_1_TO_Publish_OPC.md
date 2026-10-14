# AR_SERVER_0_1_TO_Publish_OPC

![AR_SERVER_0_1_TO_Publish_OPC_network](./AR_SERVER_0_1_TO_Publish_OPC_network.svg)

* * * * * * * * * *

## Introduction

`AR_SERVER_0_1_TO_Publish_OPC` receives a `REAL` value as the argument of a remote OPC-UA method call (`AR_SERVER_0_1`) and immediately publishes it locally (`AR_PUBLISH_1`). The block has no VT connection and is a pure forwarding bridge for stations without a VT: a VT input field on another station or a web dashboard calls the same method - and only on real input. A typical example is pressure reference values (B20).

![AR_SERVER_0_1_TO_Publish_OPC](AR_SERVER_0_1_TO_Publish_OPC.svg)

## Function Blocks (FBs) Used

### Sub-blocks: AR_SERVER_0_1_TO_Publish_OPC

- **Type**: SubAppType
- **Internal FBs used**:
    - **RECEIVE**: `adapter::net::AR_SERVER_0_1` (`QI` from outside) - creates the OPC-UA method and provides the received `REAL` argument on the adapter `OUT`
    - **PUBLISH**: `adapter::net::AR_PUBLISH_1` (`QI` from outside) - publishes the value under the local address
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_2` - combines the `QO` of `RECEIVE` and `PUBLISH`
- **Functionality**: The value from the method call is passed directly to the publisher without intermediate logic.

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request; goes to `RECEIVE.INIT`

### **Event Outputs**

- **INITO** (`EInit`): initialization confirm; comes from `PUBLISH.INITO`

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): Quality Input - TRUE enables the service; goes to `RECEIVE.QI` and `PUBLISH.QI`
- **ID_METHOD** (`WSTRING`): Locally created OPC-UA method (`ACTION=CREATE_METHOD`, 1 `REAL` argument) - callable from another station (`AR_CLIENT_1_0`) and/or directly from the OPC dashboard
- **ID_WRITE** (`WSTRING`): Local publish address (`REAL`, `ACTION=PUBLISH`) for the received value - typically read back by a local self-subscribe

### **Data Outputs**

- **QO** (`BOOL`): Quality Output - TRUE if the service was executed successfully (AND of `RECEIVE.QO` and `PUBLISH.QO`)

## Program Flow and Connections

- `ID_METHOD` → `RECEIVE.ID`
- `ID_WRITE` → `PUBLISH.ID`
- `INIT` → `RECEIVE.INIT`
- `RECEIVE.INITO` → `PUBLISH.INIT` (the publisher is initialised as soon as the method has been created)
- `PUBLISH.INITO` → `INITO` and → `AND_QO.REQ`
- `QI` → `RECEIVE.QI` and `PUBLISH.QI`
- `RECEIVE.QO` → `AND_QO.IN1`, `PUBLISH.QO` → `AND_QO.IN2`, `AND_QO.OUT` → `QO`
- `RECEIVE.OUT` → `PUBLISH.IN` (adapter connection of the `REAL` value)

## Technical Details

- No VT connection: intended for sensor stations without their own VT (e.g. STG4) that should nevertheless accept editable reference values from another station (VT) or from the dashboard
- The method is called only on real input, there is no cyclic synchronisation
- Version 1.0 (2026-09-23): bundles `AR_SERVER_0_1` (receive) and `AR_PUBLISH_1` (local forwarding)

## Application Scenarios

- Pressure reference values (B20) of a station without a VT, set by another station or by the web dashboard
- In general: accepting a `REAL` setpoint via an OPC-UA method call and providing it locally as a publish value

## Summary

`AR_SERVER_0_1_TO_Publish_OPC` encapsulates `AR_SERVER_0_1` and `AR_PUBLISH_1` into a forwarding bridge that publishes a `REAL` value received via an OPC-UA method locally.

# Attribute_IDA

![Attribute_IDA_network](./Attribute_IDA_network.svg)

* * * * * * * * * *

## Introduction

`Attribute_IDA` is an input service interface function block for double word attribute data (`DWORD`). It encapsulates [`Attribute_ID`](./Attribute_ID.md) and provides the read attribute value of a VT object (object ID `u16ObjId`, attribute ID `u8AID`) not via a data output but via the adapter plug `IN` (`AD`). Event and value are bundled in one adapter connection.

![Attribute_IDA](Attribute_IDA.svg)

## Function Blocks (FBs) Used

### Sub-blocks: Attribute_IDA

- **Type**: Composite function block
- **Internal FBs used**:
    - **Attribute_ID**: `isobus::UT::io::Attribute::Attribute_ID` - reads the attribute of the VT object
- **Import**: `isobus::UT::Q::const::IDs::ID_NULL`

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): Service initialization
    - Associated with: `QI`, `PARAMS`, `u16ObjId`, `u8AID`
- **REQ**: Service request
    - Associated with: `QI`

### **Event Outputs**

- **INITO** (`EInit`): Initialization confirmation
    - Associated with: `QO`, `STATUS`

### **Data Inputs**

- **QI** (`BOOL`): Event input qualifier
- **PARAMS** (`STRING`): Service parameters
- **u16ObjId** (`UINT`, initial value `ID_NULL`): Object ID
- **u8AID** (`USINT`): Attribute ID of the object

### **Data Outputs**

- **QO** (`BOOL`): Event output qualifier
- **STATUS** (`STRING`): Service status

### **Adapters**

- **IN** (plug): `adapter::types::unidirectional::AD` - input data from the resource (event `E1`, data `D1` with the `DWORD` attribute value)

## Program Flow and Connections

- `INIT` → `Attribute_ID.INIT`; `REQ` → `Attribute_ID.REQ`; `Attribute_ID.INITO` → `INITO`
- `Attribute_ID.IND` → `IN.E1` and `Attribute_ID.CNF` → `IN.E1`
- `u16ObjId`, `u8AID`, `PARAMS`, `QI` → like-named inputs of `Attribute_ID`
- `Attribute_ID.IN` → `IN.D1`
- `Attribute_ID.STATUS` → `STATUS`; `Attribute_ID.QO` → `QO`

## Technical Details

- Both `IND` and `CNF` of the inner block trigger `IN.E1`; the value is present at `IN.D1` in each case
- The adapter bundles event and data flow in one connection line
- Version 1.0 (2026-09-15)

## Application Scenarios

- Reading attribute values of a VT object and further processing via `AD` adapters, e.g. via `AD_TO_AX` converters

## ⚖️ Comparison with Similar Blocks

- [Attribute_ID](./Attribute_ID.md) provides `IN` as a data output and `CNF`/`IND` as event outputs, whereas `Attribute_IDA` bundles both in the adapter `IN`.

## Summary

`Attribute_IDA` provides the attribute value of a VT object as a `DWORD` via an `AD` adapter.

# AIS2_TO_AIS

![AIS2_TO_AIS_network](./AIS2_TO_AIS_network.svg)

* * * * * * * * * *

## Introduction

`AIS2_TO_AIS` is a composite function block that converts a **bidirectional** `AIS2` adapter into a **unidirectional** `AIS` adapter. The incoming side (`EO1`/`DO1`) of the bidirectional adapter is output as a plain unidirectional signal via `AIS_OUT`; via `AIS_IN` a value is written onto the outgoing side (`EI1`/`DI1`) of the bidirectional adapter.

![AIS2_TO_AIS](AIS2_TO_AIS.svg)

## Interface Structure

### **Event Inputs**

*No direct event inputs - events arrive via the adapter sockets/plugs*

### **Event Outputs**

*No direct event outputs*

### **Data Inputs**

*No data inputs*

### **Data Outputs**

*No data outputs*

### **Adapters**

- **AIS2_BIDIR** (socket): Bidirectional adapter of type `adapter::types::bidirectional::AIS2` - the incoming side is forwarded to `AIS_OUT`, the outgoing side is driven by `AIS_IN`
- **AIS_IN** (socket): Unidirectional adapter of type `adapter::types::unidirectional::AIS` - unidirectional source
- **AIS_OUT** (plug): Unidirectional adapter of type `adapter::types::unidirectional::AIS` - mirrors the incoming (indication) side of `AIS2_BIDIR`

## How it works

1. `AIS2_BIDIR.EO1` is forwarded to `AIS_OUT.E1` and `AIS2_BIDIR.DO1` to `AIS_OUT.D1`.
2. `AIS_IN.E1` is forwarded to `AIS2_BIDIR.EI1` and `AIS_IN.D1` to `AIS2_BIDIR.DI1`.

## Technical Details

- Pure event/data connections (`FBNetwork`), no logic of its own and no internal state
- Part of the conversion family (analogous to `AR2`/`AB2`/`AE2`); the data type is `STRING`

## Application Scenarios

- Connecting a unidirectional `AIS` consumer or producer to a bidirectional `AIS2` adapter

## Summary

`AIS2_TO_AIS` separates the two directions of a bidirectional `AIS2` adapter into a unidirectional output (`AIS_OUT`) and a unidirectional input (`AIS_IN`).

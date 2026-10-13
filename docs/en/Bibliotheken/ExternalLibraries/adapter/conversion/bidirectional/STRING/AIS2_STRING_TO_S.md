# AIS2_STRING_TO_S

![AIS2_STRING_TO_S_network](./AIS2_STRING_TO_S_network.svg)

* * * * * * * * * *

## Introduction

`AIS2_STRING_TO_S` is a composite function block that connects a bidirectional `AIS2` adapter (plug `AIS2_OUT`) to a classic event/data interface of type `STRING`. The event `REQ` with the data input `OUT` is written to the adapter; event and value coming from the adapter appear as `CNF` with the data output `IN`. The block belongs to the conversion family (analogous to `AR2`/`AB2`/`AE2`).

![AIS2_STRING_TO_S](AIS2_STRING_TO_S.svg)

## Interface Structure

### **Event Inputs**

- **REQ**: Request to the adapter (associated data: `OUT`)

### **Event Outputs**

- **CNF**: Confirmation from the adapter (associated data: `IN`)

### **Data Inputs**

- **OUT** (`STRING`): Value sent to the adapter

### **Data Outputs**

- **IN** (`STRING`): Value received from the adapter (input data from adapter)

### **Adapters**

- **AIS2_OUT** (plug): Bidirectional adapter of type `adapter::types::bidirectional::AIS2` (adapter output)

## How it works

1. **Send**: `REQ` is routed to `AIS2_OUT.EO1` and `OUT` to `AIS2_OUT.DO1`.
2. **Receive**: `AIS2_OUT.EI1` is routed to `CNF` and `AIS2_OUT.DI1` to `IN`.

## Technical Details

- Pure event/data connections (`FBNetwork`), no logic of its own and no internal state
- The data type is `STRING` on both sides, no value conversion takes place
- Version 1.0 (2026-09-12): conversion family completed

## Application Scenarios

- Connecting a block with a simple `REQ`/`CNF` interface to a bidirectional `AIS2` adapter connection
- Bridge between event/data ports and adapter for `STRING` values

## Summary

`AIS2_STRING_TO_S` translates a simple `STRING` event/data interface 1:1 into a bidirectional `AIS2` adapter and back.

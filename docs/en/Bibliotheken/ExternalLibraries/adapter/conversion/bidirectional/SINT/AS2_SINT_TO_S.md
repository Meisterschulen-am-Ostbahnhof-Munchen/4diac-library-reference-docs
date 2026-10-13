# AS2_SINT_TO_S

![AS2_SINT_TO_S_network](./AS2_SINT_TO_S_network.svg)

* * * * * * * * * *

## Introduction

`AS2_SINT_TO_S` is a composite function block that mediates between classic event/data connections of type `SINT` and a **bidirectional** `AS2` adapter. On `REQ`, the input `OUT` is written onto the outgoing side of the adapter plug `AS2_OUT`; the value reported via the adapter's backward direction appears at output `IN` and is acknowledged with `CNF`.

![AS2_SINT_TO_S](AS2_SINT_TO_S.svg)

## Interface Structure

### **Event Inputs**

- **REQ**: Request to the adapter. Associated data: `OUT`

### **Event Outputs**

- **CNF**: Confirmation from the adapter. Associated data: `IN`

### **Data Inputs**

- **OUT** (`SINT`): Data sent to the adapter

### **Data Outputs**

- **IN** (`SINT`): Data received from the adapter

### **Adapters**

- **AS2_OUT** (plug): Bidirectional adapter of type `adapter::types::bidirectional::AS2` (adapter output)

## How it works

1. `REQ` is forwarded to `AS2_OUT.EO1`, `OUT` to `AS2_OUT.DO1`.
2. `AS2_OUT.EI1` is forwarded to `CNF`, `AS2_OUT.DI1` to `IN`.

## Technical Details

- Pure event/data connections (`FBNetwork`), no logic of its own and no internal state
- Part of the conversion family (analogous to `AR2`/`AB2`/`AE2`)

## Application Scenarios

- Connecting `SINT` values from classic function block networks to a bidirectional `AS2` adapter

## Summary

`AS2_SINT_TO_S` maps `SINT` event/data connections 1:1 onto the two directions of an `AS2` adapter.

# AUDI2_UDINT_TO_I

![AUDI2_UDINT_TO_I_network](./AUDI2_UDINT_TO_I_network.svg)

* * * * * * * * * *

## Introduction

`AUDI2_UDINT_TO_I` is a composite function block that mediates between classic event/data connections of type `UDINT` and a **bidirectional** `AUDI2` adapter. On `REQ`, the input `OUT` is written onto the outgoing side of the adapter plug `AUDI2_OUT`; the value reported via the adapter's backward direction appears at output `IN` and is acknowledged with `CNF`.

![AUDI2_UDINT_TO_I](AUDI2_UDINT_TO_I.svg)

## Interface Structure

### **Event Inputs**

- **REQ**: Request to the adapter. Associated data: `OUT`

### **Event Outputs**

- **CNF**: Confirmation from the adapter. Associated data: `IN`

### **Data Inputs**

- **OUT** (`UDINT`): Data sent to the adapter

### **Data Outputs**

- **IN** (`UDINT`): Data received from the adapter

### **Adapters**

- **AUDI2_OUT** (plug): Bidirectional adapter of type `adapter::types::bidirectional::AUDI2` (adapter output)

## How it works

1. `REQ` is forwarded to `AUDI2_OUT.EO1`, `OUT` to `AUDI2_OUT.DO1`.
2. `AUDI2_OUT.EI1` is forwarded to `CNF`, `AUDI2_OUT.DI1` to `IN`.

## Technical Details

- Pure event/data connections (`FBNetwork`), no logic of its own and no internal state
- Part of the conversion family (analogous to `AR2`/`AB2`/`AE2`)

## Application Scenarios

- Connecting `UDINT` values from classic function block networks to a bidirectional `AUDI2` adapter

## Summary

`AUDI2_UDINT_TO_I` maps `UDINT` event/data connections 1:1 onto the two directions of an `AUDI2` adapter.

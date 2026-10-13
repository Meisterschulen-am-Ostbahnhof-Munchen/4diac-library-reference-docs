# AUDI2_I_TO_UDINT

![AUDI2_I_TO_UDINT_network](./AUDI2_I_TO_UDINT_network.svg)

* * * * * * * * * *

## Introduction

`AUDI2_I_TO_UDINT` is a composite function block that maps the bidirectional adapter [AUDI2](../../../types/bidirectional/UDINT/AUDI2.md) onto plain event and UDINT data ports. It belongs to the conversion family analogous to AR2/AB2/AE2.

![AUDI2_I_TO_UDINT](AUDI2_I_TO_UDINT.svg)

## Interface Structure

### **Event Inputs**

- **REQ** (Request to Adapter): Sends the data `OUT` through the adapter (associated with `OUT`)

### **Event Outputs**

- **CNF** (Confirmation from Adapter): Triggered when the adapter delivers an event on its incoming side (associated with `IN`)

### **Data Inputs**

- **OUT** (UDINT): Data sent to the adapter

### **Data Outputs**

- **IN** (UDINT): Data received from the adapter

### **Adapters**

- **AUDI2_IN** (Socket): Adapter of type `adapter::types::bidirectional::AUDI2`

## Functionality

The block consists of wiring only, without any logic of its own:

- `REQ` → `AUDI2_IN.EI1` and `OUT` → `AUDI2_IN.DI1`: The request event stream together with its data is published onto the outgoing side of the adapter.
- `AUDI2_IN.EO1` → `CNF` and `AUDI2_IN.DO1` → `IN`: Whatever is reported on the incoming side of the adapter appears as event `CNF` with the data `IN`.

## Technical Details

- Stateless composite block without algorithms
- Every destination variable has exactly one writer
- The adapter data is passed through unchanged, no value conversion takes place

## Application Scenarios

- Connecting classic event/data ports to a bidirectional AUDI2 adapter
- Integrating blocks with UDINT ports into adapter-based networks

## Conclusion

`AUDI2_I_TO_UDINT` maps the bidirectional AUDI2 adapter onto the ports `REQ`/`OUT` and `CNF`/`IN` without any logic.

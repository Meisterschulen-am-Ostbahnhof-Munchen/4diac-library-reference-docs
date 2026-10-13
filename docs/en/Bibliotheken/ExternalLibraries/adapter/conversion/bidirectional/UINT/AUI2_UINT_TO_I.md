# AUI2_UINT_TO_I

![AUI2_UINT_TO_I_network](./AUI2_UINT_TO_I_network.svg)

* * * * * * * * * *

## Introduction

`AUI2_UINT_TO_I` is a composite function block that maps the plain event and `UINT` data ports `REQ`/`OUT` and `CNF`/`IN` onto the bidirectional adapter [`AUI2`](../../../types/bidirectional/UINT/AUI2.md) (as a plug). It belongs to the conversion family analogous to AR2/AB2/AE2.

![AUI2_UINT_TO_I](AUI2_UINT_TO_I.svg)

## Interface Structure

### **Event Inputs**

- **REQ** (request to adapter): sends the data `OUT` through the adapter (associated with `OUT`)

### **Event Outputs**

- **CNF** (confirmation from adapter): fires when the adapter delivers an event on its incoming side (associated with `IN`)

### **Data Inputs**

- **OUT** (UINT): data that is sent to the adapter

### **Data Outputs**

- **IN** (UINT): data received from the adapter

### **Adapters**

- **AUI2_OUT** (plug): adapter of type `adapter::types::bidirectional::AUI2`

## Functionality

The block consists of wiring only, without any logic of its own:

- `REQ` → `AUI2_OUT.EO1` and `OUT` → `AUI2_OUT.DO1`: the request event stream together with its data is published onto the outgoing side of the adapter.
- `AUI2_OUT.EI1` → `CNF` and `AUI2_OUT.DI1` → `IN`: whatever is reported on the incoming side of the adapter appears as the event `CNF` with the data `IN`.

## Technical Details

- Stateless composite block without algorithms
- Every destination variable has exactly one writer
- The adapter data is passed through unchanged; no value conversion takes place
- The adapter is used as a plug here, so the block sits at the start of an AUI2 connection

## Application Scenarios

- Connecting classic event/data ports to a bidirectional AUI2 adapter
- Integrating blocks with `UINT` ports into adapter-based networks

## Conclusion

`AUI2_UINT_TO_I` maps the ports `REQ`/`OUT` and `CNF`/`IN` onto the bidirectional AUI2 adapter without any logic.

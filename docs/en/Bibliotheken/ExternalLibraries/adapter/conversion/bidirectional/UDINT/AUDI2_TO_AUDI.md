# AUDI2_TO_AUDI

![AUDI2_TO_AUDI_network](./AUDI2_TO_AUDI_network.svg)

* * * * * * * * * *

## Introduction

The AUDI2_TO_AUDI function block is the mirror image of [AUDI_TO_AUDI2](AUDI_TO_AUDI2.md): here the bidirectional [AUDI2](../../../types/bidirectional/UDINT/AUDI2.md) tap sits as a socket on the left (input) side, complemented by a second unidirectional [AUDI](../../../types/unidirectional/UDINT/AUDI.md) socket and a unidirectional AUDI plug.

![AUDI2_TO_AUDI](AUDI2_TO_AUDI.svg)

## Interface Structure

### **Event Inputs**

The function block has no direct event inputs — communication happens exclusively through the adapters.

### **Event Outputs**

The function block has no direct event outputs.

### **Data Inputs**

The function block has no direct data inputs.

### **Data Outputs**

The function block has no direct data outputs.

### **Adapters**

- **AUDI2_BIDIR** (Socket): bidirectional tap whose incoming side is forwarded to `AUDI_OUT` and whose outgoing side is fed by `AUDI_IN`, of type `adapter::types::bidirectional::AUDI2`
- **AUDI_IN** (Socket): unidirectional UDINT source, of type `adapter::types::unidirectional::AUDI`
- **AUDI_OUT** (Plug): mirrors the incoming (indication) side of `AUDI2_BIDIR` as a unidirectional signal, of type `adapter::types::unidirectional::AUDI`

## Functionality

The block operates in two directions at once: whatever the socket `AUDI2_BIDIR` receives on its indication side (`EO1`/`DO1` — i.e. whatever a bidirectional peer attached to AUDI2_BIDIR reports) is output unchanged as a standalone unidirectional signal via the plug `AUDI_OUT`. Conversely, whatever arrives at the socket `AUDI_IN` (`E1`/`D1`) is published onto the request side of `AUDI2_BIDIR` (`EI1`/`DI1`) — so AUDI2_BIDIR forwards the same signal to the attached bidirectional peer.

## Technical Details

- Three adapters instead of two: the direction received from the tap and the separately injected direction are deliberately kept apart rather than mixed
- Pure wiring, no logic or state, every destination variable has exactly one writer
- `AUDI_OUT` only reflects what is reported via `AUDI2_BIDIR` by an attached bidirectional peer — without such a peer, `AUDI_OUT` stays idle

## State Overview

The block is stateless:

- AUDI2_BIDIR.EO1 → AUDI_OUT.E1, AUDI2_BIDIR.DO1 → AUDI_OUT.D1
- AUDI_IN.E1 → AUDI2_BIDIR.EI1, AUDI_IN.D1 → AUDI2_BIDIR.DI1

## Application Scenarios

- Reading the status of a bidirectional AUDI2 peer as a plain unidirectional AUDI signal, combined with injecting a separate unidirectional command signal
- Systems where a bidirectional peer needs to be monitored without converting the rest of the control logic to AUDI2
- Bridging a bidirectional AUDI2 zone and a unidirectional AUDI zone

## ⚖️ Comparison with Similar Blocks

[AUDI_TO_AUDI2](AUDI_TO_AUDI2.md) is the mirror image of this block — there, `AUDI2_BIDIR` sits as a plug on the right side.

## Conclusion

AUDI2_TO_AUDI is not a simple converter but a tap point: it makes the state of a bidirectional AUDI2 peer visible as a standalone unidirectional AUDI signal while simultaneously injecting a separate unidirectional signal into the bidirectional side.

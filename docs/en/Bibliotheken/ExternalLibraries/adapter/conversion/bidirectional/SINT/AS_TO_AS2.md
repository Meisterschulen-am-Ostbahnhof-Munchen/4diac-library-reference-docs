# AS_TO_AS2

![AS_TO_AS2_network](./AS_TO_AS2_network.svg)

* * * * * * * * * *

## Introduction

The `AS_TO_AS2` function block inserts a bidirectional [`AS2`](../../../types/bidirectional/SINT/AS2.md) tap into a unidirectional [`AS`](../../../types/unidirectional/SINT/AS.md) chain (SINT). It uses **three** adapters: an incoming unidirectional AS signal is published onto the bidirectional AS2 interface, and whatever AS2 returns on its request side is output again as a standalone unidirectional AS signal.

![AS_TO_AS2](AS_TO_AS2.svg)

## Interface Structure

### **Event Inputs**

The function block has no direct event inputs.

### **Event Outputs**

The function block has no direct event outputs.

### **Data Inputs**

The function block has no direct data inputs.

### **Data Outputs**

The function block has no direct data outputs.

### **Adapters**

- **AS_IN** (socket): unidirectional SINT source, type `adapter::types::unidirectional::AS`
- **AS2_BIDIR** (plug): bidirectional tap whose outgoing side is fed by `AS_IN` and whose incoming side is forwarded to `AS_OUT`, type `adapter::types::bidirectional::AS2`
- **AS_OUT** (plug): mirrors the incoming (request) side of `AS2_BIDIR` as a unidirectional signal, type `adapter::types::unidirectional::AS`

## Functionality

The block operates in two directions at once: whatever arrives at the socket `AS_IN` (`E1`/`D1`) is published directly onto the outgoing side of `AS2_BIDIR` (`EO1`/`DO1`). Conversely, whatever `AS2_BIDIR` receives on its request side (`EI1`/`DI1`) is output unchanged as a standalone unidirectional signal via the plug `AS_OUT`. `AS2_BIDIR` thus acts as a tap point that does not break the unidirectional chain but extends it with a bidirectional observation/injection point.

## Technical Details

- Three adapters instead of two: the incoming signal direction and the direction coming back from the tap are deliberately kept separate
- Pure wiring, no logic or state, every destination variable has exactly one writer
- `AS_OUT` only reflects what comes back via `AS2_BIDIR` from an attached bidirectional peer; without such a peer, `AS_OUT` stays idle

## State Overview

The block is stateless:

- AS_IN.E1 → AS2_BIDIR.EO1, AS_IN.D1 → AS2_BIDIR.DO1
- AS2_BIDIR.EI1 → AS_OUT.E1, AS2_BIDIR.DI1 → AS_OUT.D1

## Application Scenarios

- Retrofitting a bidirectional diagnostic or control point into an existing unidirectional AS signal chain without breaking it
- Bridging a unidirectional AS zone and a bidirectional AS2 zone

## ⚖️ Comparison with Similar Blocks

The counterpart in the opposite direction is the block `AS2_TO_AS` (there, `AS2_BIDIR` is a socket on the left side). The blocks of this conversion family exist analogously for the adapters `AR`/`AR2`, `AB`/`AB2` and `AE`/`AE2`.

## Conclusion

`AS_TO_AS2` is not a simple converter but a tap point: it makes a unidirectional AS signal visible at a bidirectional AS2 point without breaking the original signal chain, while forwarding the request signal received there as a standalone AS signal.

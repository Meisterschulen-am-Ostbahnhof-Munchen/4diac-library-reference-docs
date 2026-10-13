# AIS_TO_AIS2

![AIS_TO_AIS2_network](./AIS_TO_AIS2_network.svg)

* * * * * * * * * *

## Introduction

The `AIS_TO_AIS2` function block inserts a bidirectional [`AIS2`](../../../types/bidirectional/STRING/AIS2.md) tap into a unidirectional [`AIS`](../../../types/unidirectional/STRING/AIS.md) chain (STRING). It uses **three** adapters: an incoming unidirectional AIS signal is published onto the bidirectional AIS2 interface, and whatever AIS2 returns on its request side is output again as a standalone unidirectional AIS signal.

![AIS_TO_AIS2](AIS_TO_AIS2.svg)

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

- **AIS_IN** (socket): unidirectional STRING source, type `adapter::types::unidirectional::AIS`
- **AIS2_BIDIR** (plug): bidirectional tap whose outgoing side is fed by `AIS_IN` and whose incoming side is forwarded to `AIS_OUT`, type `adapter::types::bidirectional::AIS2`
- **AIS_OUT** (plug): mirrors the incoming (request) side of `AIS2_BIDIR` as a unidirectional signal, type `adapter::types::unidirectional::AIS`

## Functionality

The block operates in two directions at once: whatever arrives at the socket `AIS_IN` (`E1`/`D1`) is published directly onto the outgoing side of `AIS2_BIDIR` (`EO1`/`DO1`). Conversely, whatever `AIS2_BIDIR` receives on its request side (`EI1`/`DI1`) is output unchanged as a standalone unidirectional signal via the plug `AIS_OUT`. `AIS2_BIDIR` thus acts as a tap point that does not break the unidirectional chain but extends it with a bidirectional observation/injection point.

## Technical Details

- Three adapters instead of two: the incoming signal direction and the direction coming back from the tap are deliberately kept separate
- Pure wiring, no logic or state, every destination variable has exactly one writer
- `AIS_OUT` only reflects what comes back via `AIS2_BIDIR` from an attached bidirectional peer; without such a peer, `AIS_OUT` stays idle

## State Overview

The block is stateless:

- AIS_IN.E1 → AIS2_BIDIR.EO1, AIS_IN.D1 → AIS2_BIDIR.DO1
- AIS2_BIDIR.EI1 → AIS_OUT.E1, AIS2_BIDIR.DI1 → AIS_OUT.D1

## Application Scenarios

- Retrofitting a bidirectional diagnostic or control point into an existing unidirectional AIS signal chain without breaking it
- Bridging a unidirectional AIS zone and a bidirectional AIS2 zone

## ⚖️ Comparison with Similar Blocks

The counterpart in the opposite direction is the block `AIS2_TO_AIS` (there, `AIS2_BIDIR` is a socket on the left side). The blocks of this conversion family exist analogously for the adapters `AR`/`AR2`, `AB`/`AB2` and `AE`/`AE2`.

## Conclusion

`AIS_TO_AIS2` is not a simple converter but a tap point: it makes a unidirectional AIS signal visible at a bidirectional AIS2 point without breaking the original signal chain, while forwarding the request signal received there as a standalone AIS signal.

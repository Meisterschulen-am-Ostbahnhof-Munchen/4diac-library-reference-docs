# AUI_TO_AUI2

![AUI_TO_AUI2_network](./AUI_TO_AUI2_network.svg)

* * * * * * * * * *

## Introduction

The `AUI_TO_AUI2` function block inserts a bidirectional [`AUI2`](../../../types/bidirectional/UINT/AUI2.md) tap into a unidirectional [`AUI`](../../../types/unidirectional/UINT/AUI.md) chain (UINT). It uses **three** adapters: an incoming unidirectional AUI signal is published onto the bidirectional AUI2 interface, and whatever AUI2 returns on its request side is output again as a standalone unidirectional AUI signal.

![AUI_TO_AUI2](AUI_TO_AUI2.svg)

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

- **AUI_IN** (socket): unidirectional UINT source, type `adapter::types::unidirectional::AUI`
- **AUI2_BIDIR** (plug): bidirectional tap whose outgoing side is fed by `AUI_IN` and whose incoming side is forwarded to `AUI_OUT`, type `adapter::types::bidirectional::AUI2`
- **AUI_OUT** (plug): mirrors the incoming (request) side of `AUI2_BIDIR` as a unidirectional signal, type `adapter::types::unidirectional::AUI`

## Functionality

The block operates in two directions at once: whatever arrives at the socket `AUI_IN` (`E1`/`D1`) is published directly onto the outgoing side of `AUI2_BIDIR` (`EO1`/`DO1`). Conversely, whatever `AUI2_BIDIR` receives on its request side (`EI1`/`DI1`) is output unchanged as a standalone unidirectional signal via the plug `AUI_OUT`. `AUI2_BIDIR` thus acts as a tap point that does not break the unidirectional chain but extends it with a bidirectional observation/injection point.

## Technical Details

- Three adapters instead of two: the incoming signal direction and the direction coming back from the tap are deliberately kept separate
- Pure wiring, no logic or state, every destination variable has exactly one writer
- `AUI_OUT` only reflects what comes back via `AUI2_BIDIR` from an attached bidirectional peer; without such a peer, `AUI_OUT` stays idle

## State Overview

The block is stateless:

- AUI_IN.E1 → AUI2_BIDIR.EO1, AUI_IN.D1 → AUI2_BIDIR.DO1
- AUI2_BIDIR.EI1 → AUI_OUT.E1, AUI2_BIDIR.DI1 → AUI_OUT.D1

## Application Scenarios

- Retrofitting a bidirectional diagnostic or control point into an existing unidirectional AUI signal chain without breaking it
- Bridging a unidirectional AUI zone and a bidirectional AUI2 zone

## ⚖️ Comparison with Similar Blocks

The counterpart in the opposite direction is the block `AUI2_TO_AUI` (there, `AUI2_BIDIR` is a socket on the left side). The blocks of this conversion family exist analogously for the adapters `AR`/`AR2`, `AB`/`AB2` and `AE`/`AE2`.

## Conclusion

`AUI_TO_AUI2` is not a simple converter but a tap point: it makes a unidirectional AUI signal visible at a bidirectional AUI2 point without breaking the original signal chain, while forwarding the request signal received there as a standalone AUI signal.

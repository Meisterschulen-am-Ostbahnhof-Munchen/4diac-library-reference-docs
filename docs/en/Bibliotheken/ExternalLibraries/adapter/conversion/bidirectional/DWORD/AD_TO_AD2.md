# AD_TO_AD2

![AD_TO_AD2](AD_TO_AD2.svg)

![AD_TO_AD2_network](./AD_TO_AD2_network.svg)

* * * * * * * * * *

## Introduction

The AD_TO_AD2 function block inserts a bidirectional [AD2](../../../types/bidirectional/DWORD/AD2.md) tap into a unidirectional [AD](../../../types/unidirectional/DWORD/AD.md) chain. Rather than a simple 1:1 conversion, it uses **three** adapters: an incoming unidirectional AD signal is both published onto the bidirectional AD2 interface and — whatever AD2 returns on its request side — output again as a standalone unidirectional AD signal.

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

- **AD_IN** (Socket): unidirectional DWORD source, of type `adapter::types::unidirectional::AD`
- **AD2_BIDIR** (Plug): bidirectional tap whose outgoing side is fed by `AD_IN` and whose incoming side is forwarded to `AD_OUT`, of type `adapter::types::bidirectional::AD2`
- **AD_OUT** (Plug): mirrors the incoming (request) side of `AD2_BIDIR` as a unidirectional signal, of type `adapter::types::unidirectional::AD`

## Functionality

The block operates in two directions at once: whatever arrives at the socket `AD_IN` (`E1`/`D1`) is published directly onto the outgoing side of `AD2_BIDIR` (`EO1`/`DO1`) — so AD2_BIDIR reports outward the same signal coming in via AD_IN. Conversely, whatever `AD2_BIDIR` receives on its request side (`EI1`/`DI1` — i.e. whatever a bidirectional peer attached to AD2_BIDIR "sends back") is output unchanged as a standalone unidirectional signal via the plug `AD_OUT`. AD2_BIDIR thus acts as a tap point that doesn't break the unidirectional chain but extends it with a bidirectional observation/injection point.

## Technical Details

- Three adapters instead of two: the incoming signal direction and the direction coming back from the tap are deliberately kept separate rather than mixed
- Pure wiring, no logic or state, every destination variable has exactly one writer
- `AD_OUT` only reflects what comes back via `AD2_BIDIR` from an attached bidirectional peer — without such a peer, `AD_OUT` stays idle

## State Overview

The block is stateless:

- AD_IN.E1 → AD2_BIDIR.EO1, AD_IN.D1 → AD2_BIDIR.DO1
- AD2_BIDIR.EI1 → AD_OUT.E1, AD2_BIDIR.DI1 → AD_OUT.D1

## Application Scenarios

- Retrofitting a bidirectional diagnostic or control point into an existing unidirectional AD signal chain without breaking it
- Systems where a bidirectional peer (e.g. an operator panel) needs to both display the status of a unidirectional byte signal source and send commands back
- Bridging a unidirectional AD zone and a bidirectional AD2 zone

## ⚖️ Comparison with Similar Blocks

AD_TO_AD2 belongs to the conversion family of tap blocks (analogous to [AB_TO_AB2](../BYTE/AB_TO_AB2.md) for BYTE): a unidirectional [AD](../../../types/unidirectional/DWORD/AD.md) signal is tapped at a bidirectional [AD2](../../../types/bidirectional/DWORD/AD2.md) point.

## Conclusion

AD_TO_AD2 is not a simple converter but a tap point: it makes a unidirectional AD signal visible at a bidirectional AD2 point without breaking the original unidirectional signal chain, while forwarding the request signal received there as a standalone AD signal.

## 🛠️ Related Exercises

- [Uebung_094b5_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b5_AX/)


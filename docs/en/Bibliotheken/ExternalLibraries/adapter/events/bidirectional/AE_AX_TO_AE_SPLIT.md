# AE_AX_TO_AE_SPLIT

![AE_AX_TO_AE_SPLIT_network](./AE_AX_TO_AE_SPLIT_network.svg)

* * * * * * * * * *

## Introduction

`AE_AX_TO_AE_SPLIT` is the mirrored version of `AE_TO_AE_AX_SPLIT` (plug and socket swapped 1:1). The block forwards the incoming trigger of a bidirectional `AE_AX` socket onward as a unidirectional `AE` plug (trigger pass-through), and an additional `AX` socket injects an externally supplied value into the backward (state) channel of the `AE_AX` socket.

![AE_AX_TO_AE_SPLIT](AE_AX_TO_AE_SPLIT.svg)

## Interface Structure

### **Event / Data Inputs and Outputs**

No own event or data connections; all events and data travel through the adapters.

### **Adapters**

| Direction | Name | Adapter type | Comment |
| :--- | :--- | :--- | :--- |
| **Plug** (output) | `IN` | `adapter::types::unidirectional::AE` | Output (trigger, unidirectional). |
| **Socket** (input) | `OUT` | `adapter::types::bidirectional::AE_AX` | Input (trigger in, state out). |
| **Socket** (input) | `AX_OUT` | `adapter::types::unidirectional::AX` | Feeds the backward (state) channel of the `AE_AX`. |

## Functionality

The block is a composite FB without any internal function blocks; it consists only of adapter connections:

- **Event connections:** `OUT.E1` → `IN.E1` (the event arriving at the `AE_AX` socket is output unchanged at the `AE` plug); `AX_OUT.E1` → `OUT.EI1` (the event received at the `AX` socket is sent out as the backward-channel event via the `AE_AX` socket).
- **Data connection:** `AX_OUT.D1` → `OUT.DI1` (the value received at the `AX` socket is sent out as the backward-channel datum `DI1` of the `AE_AX` socket).

## Technical Details

- Pure wiring, no logic and no delay.
- The names `IN`/`OUT` are inherited from the source `AE_TO_AE_AX_SPLIT` and are therefore swapped: `OUT` is a socket here, `IN` a plug.
- The trigger direction stays unidirectional (`AE`); the backward channel is fed entirely by the `AX` socket.

## Application Scenarios

- Splitting a bidirectional `AE_AX` connection into a pure trigger path (`AE`) and a separately fed state path (`AX`).

## Summary

`AE_AX_TO_AE_SPLIT` splits an `AE_AX` socket into a forwarded `AE` trigger and a backward channel fed via `AX`.

---

### 🌐 Related topic pages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

# AE2_DELAY

![AE2_DELAY_network](./AE2_DELAY_network.svg)

* * * * * * * * * *

## Introduction

The `AE2_DELAY` function block delays events of a bidirectional [`AE2`](../../types/bidirectional/EVENT/AE2.md) adapter. Because AE2 carries events in both directions (`EO1` forward, `EI1` backward), the block has two independent delay paths: one for the forward direction (`IN` → `OUT`) using the time `PT_FWD` and one for the backward direction (`OUT` → `IN`) using the time `PT_BWD`. Each direction can be aborted through its own `STOP` socket.

![AE2_DELAY](AE2_DELAY.svg)

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

- **IN** (socket): event from the previous chain element (forward in, backward out), type `adapter::types::bidirectional::AE2`
- **OUT** (plug): delayed event to the next chain element (forward out, backward in), type `adapter::types::bidirectional::AE2`
- **STOP_FWD** (socket): aborts the running forward delay, type `adapter::types::unidirectional::AE`
- **PT_FWD** (socket): preset time for the forward direction, type `adapter::types::unidirectional::ATM`
- **STOP_BWD** (socket): aborts the running backward delay, type `adapter::types::unidirectional::AE`
- **PT_BWD** (socket): preset time for the backward direction, adjustable independently of `PT_FWD`, type `adapter::types::unidirectional::ATM`

## Functionality

The internal network consists of two instances of [`E_DELAY`](../../../../StandardLibraries/events/E_DELAY.md):

- **Forward** (`E_DELAY_FWD`): `IN.EO1` → `E_DELAY_FWD.START`, `STOP_FWD.E1` → `E_DELAY_FWD.STOP`, `PT_FWD.D1` → `E_DELAY_FWD.DT`. When the time has elapsed, `E_DELAY_FWD.EO` is output on `OUT.EO1`.
- **Backward** (`E_DELAY_BWD`): `OUT.EI1` → `E_DELAY_BWD.START`, `STOP_BWD.E1` → `E_DELAY_BWD.STOP`, `PT_BWD.D1` → `E_DELAY_BWD.DT`. When the time has elapsed, `E_DELAY_BWD.EO` is output on `IN.EI1`.

The data of the adapter is not processed; only the events `EO1`/`EI1` are passed on.

## Technical Details

- Two independent delay paths (one `E_DELAY` instance per direction), each with its own time and its own `STOP`
- The times are supplied through `ATM` sockets instead of plain input variables and can therefore be initialised cleanly via `initval_ATM` (with `INIT`/`INITO`)
- The time sockets are named `PT` (preset time) rather than `DT`, because `DT` is a reserved keyword (`DATE_AND_TIME`) in IEC 61131-3
- The block itself is stateless; the state (running timer) resides in the two `E_DELAY` instances

## Application Scenarios

- Time-shifted forwarding of events in an AE2 chain, e.g. staggered switch-on and switch-off with different times
- Decoupling of request and feedback in bidirectional event chains

## ⚖️ Comparison with Similar Blocks

For chains with separately delayed set and reset branches there is `ASR2_DELAY`, whose variant with preset times is [`ASR2_DELAY_SUB`](../../../MyLib_AX/sys/ASR2_DELAY_SUB.md). `AE2_DELAY`, in contrast, delays exactly one event per direction.

## Conclusion

`AE2_DELAY` is a pure wiring block built from two `E_DELAY` instances. It delays events of an AE2 adapter in both directions, each with its own time adjustable through an `ATM` socket, and each direction can be stopped separately.

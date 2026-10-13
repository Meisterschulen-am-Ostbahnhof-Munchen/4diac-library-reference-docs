# ASR2_DELAY

![ASR2_DELAY_network](./ASR2_DELAY_network.svg)

* * * * * * * * * *

## Introduction

`ASR2_DELAY` is a composite function block that forwards the set/reset events of a bidirectional `ASR2` adapter **with a delay** (e.g. for staggered switching on/off of several belts using [`AX_T_FF_SR_SEQ`](../unidirectional/BOOL/AX_T_FF_SR_SEQ.md)). Since `ASR2` is truly bidirectional (`EI_SET`/`EI_RESET`/`EO_SET`/`EO_RESET`), **four independent** [`E_DELAY`](../../../../StandardLibraries/events/E_DELAY.md) instances are used: set and reset, each in forward and backward direction, each with its own time and its own stop input.

![ASR2_DELAY](ASR2_DELAY.svg)

## Interface Structure

### **Event Inputs**

*No direct event inputs - events arrive via the adapter sockets/plugs*

### **Event Outputs**

*No direct event outputs*

### **Data Inputs**

*No data inputs - the times are supplied via `ATM` sockets*

### **Data Outputs**

*No data outputs*

### **Adapters**

- **IN** (socket): `adapter::types::bidirectional::ASR2` - set/reset from the previous chain element (forward in, backward out)
- **OUT** (plug): `adapter::types::bidirectional::ASR2` - delayed set/reset to the next chain element (forward out, backward in)
- **PT_SET_FWD** (socket, `adapter::types::unidirectional::ATM`): delay time SET forward (`IN.EO_SET` → `OUT.EO_SET`)
- **PT_SET_BWD** (socket, `ATM`): delay time SET backward (`OUT.EI_SET` → `IN.EI_SET`)
- **PT_RESET_FWD** (socket, `ATM`): delay time RESET forward (`IN.EO_RESET` → `OUT.EO_RESET`)
- **PT_RESET_BWD** (socket, `ATM`): delay time RESET backward (`OUT.EI_RESET` → `IN.EI_RESET`)
- **STOP_SET_FWD**, **STOP_SET_BWD**, **STOP_RESET_FWD**, **STOP_RESET_BWD** (sockets, `adapter::types::unidirectional::AE`): stop the respective delay

## Function Blocks (FBs) Used

- **E_DELAY_SET_FWD**, **E_DELAY_SET_BWD**, **E_DELAY_RESET_FWD**, **E_DELAY_RESET_BWD**: each `iec61499::events::E_DELAY`

## How it works

| Path | START | STOP | Time (`DT`) | Output |
| --- | --- | --- | --- | --- |
| SET forward | `IN.EO_SET` | `STOP_SET_FWD.E1` | `PT_SET_FWD.D1` | `OUT.EO_SET` |
| SET backward | `OUT.EI_SET` | `STOP_SET_BWD.E1` | `PT_SET_BWD.D1` | `IN.EI_SET` |
| RESET forward | `IN.EO_RESET` | `STOP_RESET_FWD.E1` | `PT_RESET_FWD.D1` | `OUT.EO_RESET` |
| RESET backward | `OUT.EI_RESET` | `STOP_RESET_BWD.E1` | `PT_RESET_BWD.D1` | `IN.EI_RESET` |

Each incoming event starts the associated `E_DELAY` instance; after the time has elapsed the event is fired at the output, unless the delay was stopped beforehand via the stop socket.

## Technical Details

- The four times are provided via `ATM` sockets and can therefore be set independently of each other and initialized via `initval_ATM` with `INIT`/`INITO` (versions 1.1/1.2). The identifiers use the prefix `PT` instead of `DT` because `DT` is a reserved keyword.
- Since version 1.2 both directions are delayed (previously only forward).

## Application Scenarios

- Staggered switching on/off of consecutive conveyor belts via an `AX_T_FF_SR_SEQ` chain

## Summary

`ASR2_DELAY` delays set and reset of an `ASR2` chain element in both directions with four separately adjustable times and four stop inputs.

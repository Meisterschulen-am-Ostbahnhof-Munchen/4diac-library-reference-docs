# SUBSCRIBE_R_TRIG

![SUBSCRIBE_R_TRIG_network](./SUBSCRIBE_R_TRIG_network.svg)

* * * * * * * * * *

## Introduction

`SUBSCRIBE_R_TRIG` monitors a `AX` value (BOOL) subscribed to via OPC UA for a rising edge and outputs it as a single event (`EO`). This allows a remotely set BOOL value to be used directly as a triggering event without having to process the level itself further.


## Function Blocks (FBs) Used

### Sub-Blocks: SUBSCRIBE_R_TRIG

- **Type**: SubAppType
- **Internal FBs Used**:

- **SUBSCRIBE**: `adapter::net::AX_SUBSCRIBE_1` (`QI=TRUE`) — subscribes to the remote BOOL value (`ID`, e.g., `Anlage_EIN_READ`).

- **AX_R_TRIG**: `adapter::events::unidirectional::AX_R_TRIG` — detects the rising edge of the subscribed value and fires `EO`.

- **How it Works**: The value received via OPC UA is directly routed to `AX_R_TRIG`, whose edge detection triggers the SubApp event `EO`.

## Program Flow and Connections

1. `ID` → `SUBSCRIBE.ID` (Data connection, hidden).

2. `SUBSCRIBE.OUT` → `AX_R_TRIG.QI`.

3. `AX_R_TRIG.EO` → `EO` (SubApp event output).


## Application Scenarios

- A remote OPC UA signal (e.g., "Plant ON") should trigger a one-time event (e.g., start an initialization) instead of being processed as a continuous level.

## Summary

`SUBSCRIBE_R_TRIG` directly combines OPC UA Subscribe with edge detection and delivers a single event on the rising edge of the remote value.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de ](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

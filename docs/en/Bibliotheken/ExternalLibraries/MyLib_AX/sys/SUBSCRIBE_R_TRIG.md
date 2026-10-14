# SUBSCRIBE_R_TRIG

![SUBSCRIBE_R_TRIG_network](./SUBSCRIBE_R_TRIG_network.svg)

* * * * * * * * * *

## Introduction

`SUBSCRIBE_R_TRIG` monitors a `AX` value (BOOL) subscribed to via OPC UA for a rising edge and outputs it as a single event (`EO`). This allows a remotely set BOOL value to be used directly as a triggering event without having to process the level itself further.


![SUBSCRIBE_R_TRIG](SUBSCRIBE_R_TRIG.svg)

## Function Blocks (FBs) Used

### Sub-Blocks: SUBSCRIBE_R_TRIG

- **Type**: SubAppType
- **Internal FBs Used**:

- **SUBSCRIBE**: `iec61499::net::SUBSCRIBE_1` — subscribes to the remote BOOL value (`ID`, e.g., `Anlage_EIN_READ`).

- **E_R_TRIG**: `iec61499::events::E_R_TRIG` — detects the rising edge of the subscribed value and fires `EO`.

- **How it Works**: Since version 2.0 the raw `SUBSCRIBE_1` and `E_R_TRIG` are wired directly (no duplicate edge memory any more): `SUBSCRIBE.IND` and `SUBSCRIBE.RD_1` go directly into `E_R_TRIG`, whose edge detection triggers the SubApp event `EO`.

- **Interface**: event input `INIT` (`EInit`), event outputs `INITO` (`EInit`) and `EO`; data inputs `QI` (`BOOL`, default `TRUE`) and `ID` (`WSTRING`); data output `QO` (`BOOL`).

## Program Flow and Connections

1. `ID` → `SUBSCRIBE.ID` (Data connection, hidden).

2. `SUBSCRIBE.IND` → `E_R_TRIG.EI`; `SUBSCRIBE.RD_1` → `E_R_TRIG.QI`.

3. `E_R_TRIG.EO` → `EO` (SubApp event output).

4. `INIT` → `SUBSCRIBE.INIT`; `SUBSCRIBE.INITO` → `INITO`.

5. `QI` → `SUBSCRIBE.QI`; `SUBSCRIBE.QO` → `QO`.

## Application Scenarios

- A remote OPC UA signal (e.g., "Plant ON") should trigger a one-time event (e.g., start an initialization) instead of being processed as a continuous level.

## Summary

`SUBSCRIBE_R_TRIG` directly combines OPC UA Subscribe with edge detection and delivers a single event on the rising edge of the remote value.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de ](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

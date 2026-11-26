# DlPgnSend

![DlPgnSend](./DlPgnSend.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Send a PGN once without an installed Tx PGN

Package: `isobus::pgn::tx`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Service Initialization
-   **REQ** (Type: `Event`): Send the PGN once (with data: `u32Pgn`, `u8Priority`, `NmDestin`, `u16DaSize`, `Data`)

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialization Confirm
-   **CNF** (Type: `Event`): Send started (see s16Status) (with data: `s16Status`, `Data`)

### **Data inputs**

-   **u32Pgn** (Type: `UDINT`): PGN - Range: 0 - 0x3FFFF
-   **u8Priority** (Type: `USINT`): Priority (0..7) 0=highest (Default: `6`)
-   **NmDestin** (Type: `isobus::pgn::ISONETEVENT_T`): Sender (s16OwnCfHnd) and receiver (s16Handle)
-   **u16DaSize** (Type: `UINT`): Number of data bytes to send (0..1785)

### **Data outputs**

-   **s16Status** (Type: `INT`): Return value of iso_DlPgnSend(): 0 = OK

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

## Extended Transport Protocol (ETP)

!!! warning "State: not yet tested"
    The following statement comes from the firmware development and has not been checked by me.

- `DlPgnSend` stays at 1785 bytes at most and does not send via ETP. For larger messages use [`AlPgnTxNew_TP`](AlPgnTxNew_TP.md), which uses ETP automatically from 1785 bytes.

Note: This page was generated from the type definition (`DlPgnSend.fbt`) and only shows the interface. A detailed description of the behavior is still missing.

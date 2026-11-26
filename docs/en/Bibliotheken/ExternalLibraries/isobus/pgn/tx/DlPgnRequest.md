# DlPgnRequest

![DlPgnRequest](./DlPgnRequest.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Send a request (PGN 59904) for a PGN

Package: `isobus::pgn::tx`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Service Initialization
-   **REQ** (Type: `Event`): Send the request (with data: `u32Pgn`, `NmDestin`)

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialization Confirm
-   **CNF** (Type: `Event`): Request sent (see s16Status) (with data: `s16Status`)

### **Data inputs**

-   **u32Pgn** (Type: `UDINT`): Requested PGN - Range: 0 - 0x3FFFF
-   **NmDestin** (Type: `isobus::pgn::ISONETEVENT_T`): Sender (s16OwnCfHnd) and receiver (s16Handle)

### **Data outputs**

-   **s16Status** (Type: `INT`): Return value of iso_DlPgnRequest(): 0 = OK

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`DlPgnRequest.fbt`) and only shows the interface. A detailed description of the behavior is still missing.

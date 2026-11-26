# FsRead

![FsRead](./FsRead.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Read a small file from the file server into a byte array

Package: `isobus::fs`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Service Initialization
-   **REQ** (Type: `Event`): Read the file (with data: `NmDestin`, `sRemotePath`, `Data`)

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialization Confirm
-   **CNF** (Type: `Event`): File read (with data: `u32Length`, `Data`)
-   **ERR** (Type: `Event`): Job failed (with data: `s16Status`, `u16FsError`, `u32Length`, `Data`)

### **Data inputs**

-   **NmDestin** (Type: `isobus::pgn::ISONETEVENT_T`): Client (s16OwnCfHnd) and file server (s16Handle)
-   **sRemotePath** (Type: `STRING`): Path on the file server, e.g. `logibus_test.txt`. Without a volume it ends up in the current directory of the server (primary volume). The CCI 1200 has no fixed `\\USB\`: a stick is named after its FAT serial number, e.g. `\\E82C-0533\`.

### **Data outputs**

-   **s16Status** (Type: `INT`): With ERR: driver return value or own error (E_BUSY, E_RANGE, E_OVERFLOW)
-   **u16FsError** (Type: `UINT`): With ERR: file server or client error (ISO 11783-13 B.9, 6400 ff. client)
-   **u32Length** (Type: `UDINT`): Number of bytes read into Data

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

## Notes on the behavior

-   Hardware test on the CCI 1200 with a USB stick (2026-11-26, verified): exercise 156 (`FsWrite`/`FsRead`) and `FsGet` passed. `FsPut` and exercise 156b are not fully tested yet.
-   Server in this test: address 0xF8, NAME function 61, manufacturer 339.
-   Paths without a volume (`logibus_test.txt`) are the recommended form. `~\` would be the manufacturer directory of the client and is still untested against the CCI.

Note: This page was generated from the type definition (`FsRead.fbt`) and only shows the interface. A detailed description of the behavior is still missing.

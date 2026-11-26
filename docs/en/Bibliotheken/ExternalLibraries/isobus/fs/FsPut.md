# FsPut

![FsPut](./FsPut.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Copy a local file to the file server

Package: `isobus::fs`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Service Initialization
-   **REQ** (Type: `Event`): Start the copy (with data: `NmDestin`, `sLocalPath`, `sRemotePath`, `bAppend`)

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialization Confirm
-   **CNF** (Type: `Event`): File copied (with data: `u32Bytes`)
-   **PROG** (Type: `Event`): Progress (with data: `u32Bytes`)
-   **ERR** (Type: `Event`): Job failed (with data: `s16Status`, `u16FsError`, `u32Bytes`)

### **Data inputs**

-   **NmDestin** (Type: `isobus::pgn::ISONETEVENT_T`): Client (s16OwnCfHnd) and file server (s16Handle)
-   **sLocalPath** (Type: `STRING`): Local source file, e.g. /data/fs/a.csv
-   **sRemotePath** (Type: `STRING`): Path on the file server, e.g. `logibus_test.txt`. Without a volume it ends up in the current directory of the server (primary volume). The CCI 1200 has no fixed `\\USB\`: a stick is named after its FAT serial number, e.g. `\\E82C-0533\`.
-   **bAppend** (Type: `BOOL`): FALSE: replace the file, TRUE: append

### **Data outputs**

-   **s16Status** (Type: `INT`): With ERR: driver return value or own error (E_BUSY, E_RANGE, E_OVERFLOW)
-   **u16FsError** (Type: `UINT`): With ERR: file server or client error (ISO 11783-13 B.9, 6400 ff. client)
-   **u32Bytes** (Type: `UDINT`): Bytes copied

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

## Notes on the behavior

-   Hardware test on the CCI 1200 with a USB stick (2026-11-26, verified): exercise 156 (`FsWrite`/`FsRead`) and `FsGet` passed. `FsPut` and exercise 156b are not fully tested yet.
-   Server in this test: address 0xF8, NAME function 61, manufacturer 339.
-   Paths without a volume (`logibus_test.txt`) are the recommended form. `~\` would be the manufacturer directory of the client and is still untested against the CCI.

Note: This page was generated from the type definition (`FsPut.fbt`) and only shows the interface. A detailed description of the behavior is still missing.

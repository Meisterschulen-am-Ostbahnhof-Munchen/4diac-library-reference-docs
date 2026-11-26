# FsGet

![FsGet](./FsGet.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Copy a file from the file server into a local file

Package: `isobus::fs`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Service Initialization
-   **REQ** (Type: `Event`): Start the copy (with data: `NmDestin`, `sRemotePath`, `sLocalPath`)

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialization Confirm
-   **CNF** (Type: `Event`): File copied (with data: `u32Bytes`)
-   **PROG** (Type: `Event`): Progress (at most every 500 ms) (with data: `u32Bytes`)
-   **ERR** (Type: `Event`): Job failed (with data: `s16Status`, `u16FsError`, `u32Bytes`)

### **Data inputs**

-   **NmDestin** (Type: `isobus::pgn::ISONETEVENT_T`): Client (s16OwnCfHnd) and file server (s16Handle)
-   **sRemotePath** (Type: `STRING`): Path on the file server, e.g. `logibus_test.txt`. Without a volume it ends up in the current directory of the server (primary volume). The CCI 1200 has no fixed `\\USB\`: a stick is named after its FAT serial number, e.g. `\\E82C-0533\`.
-   **sLocalPath** (Type: `STRING`): Local target file, e.g. /data/fs/a.csv

### **Data outputs**

-   **s16Status** (Type: `INT`): With ERR: driver return value or own error (E_BUSY, E_RANGE, E_OVERFLOW)
-   **u16FsError** (Type: `UINT`): With ERR: file server or client error (ISO 11783-13 B.9, 6400 ff. client)
-   **u32Bytes** (Type: `UDINT`): Bytes copied so far

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

## Notes on the behavior

-   Hardware test on the CCI 1200 with a USB stick (2026-11-26, verified): exercise 156 (`FsWrite`/`FsRead`) and `FsGet` passed. `FsPut` and exercise 156b are not fully tested yet.
-   Server in this test: address 0xF8, NAME function 61, manufacturer 339.
-   Paths without a volume (`logibus_test.txt`) are the recommended form. `~\` would be the manufacturer directory of the client and is still untested against the CCI.
-   `FsGet` creates missing intermediate directories under `/data/` itself, at any depth. If that is not possible, the job ends with `ERR` and `E_NOACT` (-8). The firmware creates `/data/fs` at startup.
-   Prerequisite: firmware from b-228 (host symbol `mkdir`) and the current `isobus_fs` ELF. With older firmware the new ELF does not load (information from the firmware developers).
-   Local paths remain restricted: `/data/fs/` for foreign file servers, `/data/` for logiBUS.

Note: This page was generated from the type definition (`FsGet.fbt`) and only shows the interface. A detailed description of the behavior is still missing.

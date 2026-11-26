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
-   **sRemotePath** (Type: `STRING`): Path on the file server, e.g. \\USB\data\a.csv
-   **sLocalPath** (Type: `STRING`): Local target file, e.g. /data/fs/a.csv

### **Data outputs**

-   **s16Status** (Type: `INT`): With ERR: driver return value or own error (E_BUSY, E_RANGE, E_OVERFLOW)
-   **u16FsError** (Type: `UINT`): With ERR: file server or client error (ISO 11783-13 B.9, 6400 ff. client)
-   **u32Bytes** (Type: `UDINT`): Bytes copied so far

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`FsGet.fbt`) and only shows the interface. A detailed description of the behavior is still missing.

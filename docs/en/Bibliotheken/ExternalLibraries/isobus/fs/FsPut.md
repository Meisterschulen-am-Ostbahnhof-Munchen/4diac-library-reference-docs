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
-   **sRemotePath** (Type: `STRING`): Path on the file server, e.g. \\USB\data\a.csv
-   **bAppend** (Type: `BOOL`): FALSE: replace the file, TRUE: append

### **Data outputs**

-   **s16Status** (Type: `INT`): With ERR: driver return value or own error (E_BUSY, E_RANGE, E_OVERFLOW)
-   **u16FsError** (Type: `UINT`): With ERR: file server or client error (ISO 11783-13 B.9, 6400 ff. client)
-   **u32Bytes** (Type: `UDINT`): Bytes copied

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`FsPut.fbt`) and only shows the interface. A detailed description of the behavior is still missing.

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
-   **sRemotePath** (Type: `STRING`): Path on the file server, e.g. \\USB\data\a.csv

### **Data outputs**

-   **s16Status** (Type: `INT`): With ERR: driver return value or own error (E_BUSY, E_RANGE, E_OVERFLOW)
-   **u16FsError** (Type: `UINT`): With ERR: file server or client error (ISO 11783-13 B.9, 6400 ff. client)
-   **u32Length** (Type: `UDINT`): Number of bytes read into Data

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`FsRead.fbt`) and only shows the interface. A detailed description of the behavior is still missing.

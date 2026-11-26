# FsWrite

![FsWrite](./FsWrite.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Write a byte array as file to the file server

Package: `isobus::fs`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Service Initialization
-   **REQ** (Type: `Event`): Write the file (with data: `NmDestin`, `sRemotePath`, `u32Length`, `bAppend`, `Data`)

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialization Confirm
-   **CNF** (Type: `Event`): File written (with data: `Data`)
-   **ERR** (Type: `Event`): Job failed (with data: `s16Status`, `u16FsError`, `Data`)

### **Data inputs**

-   **NmDestin** (Type: `isobus::pgn::ISONETEVENT_T`): Client (s16OwnCfHnd) and file server (s16Handle)
-   **sRemotePath** (Type: `STRING`): Path on the file server, e.g. \\USB\data\a.csv
-   **u32Length** (Type: `UDINT`): Number of bytes of Data to write
-   **bAppend** (Type: `BOOL`): FALSE: replace the file, TRUE: append

### **Data outputs**

-   **s16Status** (Type: `INT`): With ERR: driver return value or own error (E_BUSY, E_RANGE, E_OVERFLOW)
-   **u16FsError** (Type: `UINT`): With ERR: file server or client error (ISO 11783-13 B.9, 6400 ff. client)

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`FsWrite.fbt`) and only shows the interface. A detailed description of the behavior is still missing.

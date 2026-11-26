# FsGet

![FsGet](./FsGet.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Copy a file from the file server into a local file

Paket: `isobus::fs`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Service Initialization
-   **REQ** (Typ: `Event`): Start the copy (mit Daten: `NmDestin`, `sRemotePath`, `sLocalPath`)

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialization Confirm
-   **CNF** (Typ: `Event`): File copied (mit Daten: `u32Bytes`)
-   **PROG** (Typ: `Event`): Progress (at most every 500 ms) (mit Daten: `u32Bytes`)
-   **ERR** (Typ: `Event`): Job failed (mit Daten: `s16Status`, `u16FsError`, `u32Bytes`)

### **Daten-Eingänge**

-   **NmDestin** (Typ: `isobus::pgn::ISONETEVENT_T`): Client (s16OwnCfHnd) and file server (s16Handle)
-   **sRemotePath** (Typ: `STRING`): Path on the file server, e.g. \\USB\data\a.csv
-   **sLocalPath** (Typ: `STRING`): Local target file, e.g. /data/fs/a.csv

### **Daten-Ausgänge**

-   **s16Status** (Typ: `INT`): With ERR: driver return value or own error (E_BUSY, E_RANGE, E_OVERFLOW)
-   **u16FsError** (Typ: `UINT`): With ERR: file server or client error (ISO 11783-13 B.9, 6400 ff. client)
-   **u32Bytes** (Typ: `UDINT`): Bytes copied so far

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

Hinweis: Diese Seite wurde aus der Typdefinition (`FsGet.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

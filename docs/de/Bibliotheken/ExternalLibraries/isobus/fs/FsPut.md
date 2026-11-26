# FsPut

![FsPut](./FsPut.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Copy a local file to the file server

Paket: `isobus::fs`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Service Initialization
-   **REQ** (Typ: `Event`): Start the copy (mit Daten: `NmDestin`, `sLocalPath`, `sRemotePath`, `bAppend`)

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialization Confirm
-   **CNF** (Typ: `Event`): File copied (mit Daten: `u32Bytes`)
-   **PROG** (Typ: `Event`): Progress (mit Daten: `u32Bytes`)
-   **ERR** (Typ: `Event`): Job failed (mit Daten: `s16Status`, `u16FsError`, `u32Bytes`)

### **Daten-Eingänge**

-   **NmDestin** (Typ: `isobus::pgn::ISONETEVENT_T`): Client (s16OwnCfHnd) and file server (s16Handle)
-   **sLocalPath** (Typ: `STRING`): Local source file, e.g. /data/fs/a.csv
-   **sRemotePath** (Typ: `STRING`): Path on the file server, e.g. \\USB\data\a.csv
-   **bAppend** (Typ: `BOOL`): FALSE: replace the file, TRUE: append

### **Daten-Ausgänge**

-   **s16Status** (Typ: `INT`): With ERR: driver return value or own error (E_BUSY, E_RANGE, E_OVERFLOW)
-   **u16FsError** (Typ: `UINT`): With ERR: file server or client error (ISO 11783-13 B.9, 6400 ff. client)
-   **u32Bytes** (Typ: `UDINT`): Bytes copied

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

Hinweis: Diese Seite wurde aus der Typdefinition (`FsPut.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

# FsRead

![FsRead](./FsRead.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Read a small file from the file server into a byte array

Paket: `isobus::fs`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Service Initialization
-   **REQ** (Typ: `Event`): Read the file (mit Daten: `NmDestin`, `sRemotePath`, `Data`)

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialization Confirm
-   **CNF** (Typ: `Event`): File read (mit Daten: `u32Length`, `Data`)
-   **ERR** (Typ: `Event`): Job failed (mit Daten: `s16Status`, `u16FsError`, `u32Length`, `Data`)

### **Daten-Eingänge**

-   **NmDestin** (Typ: `isobus::pgn::ISONETEVENT_T`): Client (s16OwnCfHnd) and file server (s16Handle)
-   **sRemotePath** (Typ: `STRING`): Path on the file server, e.g. \\USB\data\a.csv

### **Daten-Ausgänge**

-   **s16Status** (Typ: `INT`): With ERR: driver return value or own error (E_BUSY, E_RANGE, E_OVERFLOW)
-   **u16FsError** (Typ: `UINT`): With ERR: file server or client error (ISO 11783-13 B.9, 6400 ff. client)
-   **u32Length** (Typ: `UDINT`): Number of bytes read into Data

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

Hinweis: Diese Seite wurde aus der Typdefinition (`FsRead.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

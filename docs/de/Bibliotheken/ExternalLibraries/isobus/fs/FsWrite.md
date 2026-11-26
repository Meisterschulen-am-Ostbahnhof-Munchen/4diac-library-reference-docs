# FsWrite

![FsWrite](./FsWrite.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Write a byte array as file to the file server

Paket: `isobus::fs`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Service Initialization
-   **REQ** (Typ: `Event`): Write the file (mit Daten: `NmDestin`, `sRemotePath`, `u32Length`, `bAppend`, `Data`)

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialization Confirm
-   **CNF** (Typ: `Event`): File written (mit Daten: `Data`)
-   **ERR** (Typ: `Event`): Job failed (mit Daten: `s16Status`, `u16FsError`, `Data`)

### **Daten-Eingänge**

-   **NmDestin** (Typ: `isobus::pgn::ISONETEVENT_T`): Client (s16OwnCfHnd) and file server (s16Handle)
-   **sRemotePath** (Typ: `STRING`): Pfad auf dem File Server, z. B. `logibus_test.txt`. Ohne Volume landet er im aktuellen Verzeichnis des Servers (primäres Volume). Am CCI 1200 gibt es kein festes `\\USB\`: Ein Stick heißt nach seiner FAT-Seriennummer, z. B. `\\E82C-0533\`.
-   **u32Length** (Typ: `UDINT`): Number of bytes of Data to write
-   **bAppend** (Typ: `BOOL`): FALSE: replace the file, TRUE: append

### **Daten-Ausgänge**

-   **s16Status** (Typ: `INT`): With ERR: driver return value or own error (E_BUSY, E_RANGE, E_OVERFLOW)
-   **u16FsError** (Typ: `UINT`): With ERR: file server or client error (ISO 11783-13 B.9, 6400 ff. client)

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

## Hinweise zum Verhalten

-   Hardware-Test am CCI 1200 mit USB-Stick (2026-11-26, geprüft): Übung 156 (`FsWrite`/`FsRead`) und `FsGet` bestanden. `FsPut` und Übung 156b sind noch nicht vollständig getestet.
-   Server in diesem Test: Adresse 0xF8, NAME-Funktion 61, Hersteller 339.
-   Pfade ohne Volume (`logibus_test.txt`) sind die empfohlene Form. `~\` wäre das Herstellerverzeichnis des Clients und ist gegen das CCI noch ungetestet.

Hinweis: Diese Seite wurde aus der Typdefinition (`FsWrite.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

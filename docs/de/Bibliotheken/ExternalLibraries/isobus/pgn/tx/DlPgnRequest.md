# DlPgnRequest

![DlPgnRequest](./DlPgnRequest.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Send a request (PGN 59904) for a PGN

Paket: `isobus::pgn::tx`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Service Initialization
-   **REQ** (Typ: `Event`): Send the request (mit Daten: `u32Pgn`, `NmDestin`)

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialization Confirm
-   **CNF** (Typ: `Event`): Request sent (see s16Status) (mit Daten: `s16Status`)

### **Daten-Eingänge**

-   **u32Pgn** (Typ: `UDINT`): Requested PGN - Range: 0 - 0x3FFFF
-   **NmDestin** (Typ: `isobus::pgn::ISONETEVENT_T`): Sender (s16OwnCfHnd) and receiver (s16Handle)

### **Daten-Ausgänge**

-   **s16Status** (Typ: `INT`): Return value of iso_DlPgnRequest(): 0 = OK

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

Hinweis: Diese Seite wurde aus der Typdefinition (`DlPgnRequest.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

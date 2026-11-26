# CAN_MSG_TO_STRING

![CAN_MSG_TO_STRING](./CAN_MSG_TO_STRING.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: CAN_MSG von AlPgnRxNew8B (bis 8 Byte) als STRING auslesen

Paket: `isobus::pgn`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **REQ** (Typ: `Event`): Text auslesen (mit Daten: `MSG`, `STOP_AT_NUL`)

### **Ereignis-Ausgänge**

-   **CNF** (Typ: `Event`): OUT ist gefuellt (mit Daten: `OUT`)

### **Daten-Eingänge**

-   **MSG** (Typ: `isobus::pgn::CAN_MSG`): empfangene Nachricht, gelesen werden u16DaSize Bytes (hoechstens 8)
-   **STOP_AT_NUL** (Typ: `BOOL`): TRUE: Text endet vor dem ersten 16#00 (Vorgabewert: `TRUE`)

### **Daten-Ausgänge**

-   **OUT** (Typ: `STRING`): Text, je Byte 1 Zeichen

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

Hinweis: Diese Seite wurde aus der Typdefinition (`CAN_MSG_TO_STRING.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

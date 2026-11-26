# DlPgnSend

![DlPgnSend](./DlPgnSend.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Send a PGN once without an installed Tx PGN

Paket: `isobus::pgn::tx`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Service Initialization
-   **REQ** (Typ: `Event`): Send the PGN once (mit Daten: `u32Pgn`, `u8Priority`, `NmDestin`, `u16DaSize`, `Data`)

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialization Confirm
-   **CNF** (Typ: `Event`): Send started (see s16Status) (mit Daten: `s16Status`, `Data`)

### **Daten-Eingänge**

-   **u32Pgn** (Typ: `UDINT`): PGN - Range: 0 - 0x3FFFF
-   **u8Priority** (Typ: `USINT`): Priority (0..7) 0=highest (Vorgabewert: `6`)
-   **NmDestin** (Typ: `isobus::pgn::ISONETEVENT_T`): Sender (s16OwnCfHnd) and receiver (s16Handle)
-   **u16DaSize** (Typ: `UINT`): Number of data bytes to send (0..1785)

### **Daten-Ausgänge**

-   **s16Status** (Typ: `INT`): Return value of iso_DlPgnSend(): 0 = OK

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

## Extended Transport Protocol (ETP)

!!! warning "Stand: noch nicht getestet"
    Die folgende Angabe stammt von der Firmware-Entwicklung und ist nicht von mir geprüft.

- `DlPgnSend` bleibt bei höchstens 1785 Byte und sendet nicht per ETP. Für größere Nachrichten dient [`AlPgnTxNew_TP`](AlPgnTxNew_TP.md), das ab 1785 Byte automatisch ETP nutzt.

Hinweis: Diese Seite wurde aus der Typdefinition (`DlPgnSend.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

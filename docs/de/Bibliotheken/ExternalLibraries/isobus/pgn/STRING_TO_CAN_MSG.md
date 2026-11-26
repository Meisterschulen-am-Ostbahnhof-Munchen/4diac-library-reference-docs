# STRING_TO_CAN_MSG

![STRING_TO_CAN_MSG](./STRING_TO_CAN_MSG.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: STRING (bis 8 Zeichen) in eine CAN_MSG fuer AlPgnTxNew8B packen

Paket: `isobus::pgn`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **REQ** (Typ: `Event`): Text packen (mit Daten: `IN`, `PRIO`)

### **Ereignis-Ausgänge**

-   **CNF** (Typ: `Event`): MSG ist gefuellt (mit Daten: `MSG`, `TRUNCATED`)

### **Daten-Eingänge**

-   **IN** (Typ: `STRING`): Text, je Zeichen 1 Byte
-   **PRIO** (Typ: `USINT`): Prioritaet (0..7), groesser 7 wird 7; AlPgnTxNew8B nimmt die Prioritaet vom install, nicht aus MSG (Vorgabewert: `6`)

### **Daten-Ausgänge**

-   **MSG** (Typ: `isobus::pgn::CAN_MSG`): data = Zeichen, dann 16#00 (falls Platz), Rest 16#FF; u16DaSize = 8
-   **TRUNCATED** (Typ: `BOOL`): TRUE, wenn IN laenger als 8 Zeichen war (abgeschnitten)

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

Hinweis: Diese Seite wurde aus der Typdefinition (`STRING_TO_CAN_MSG.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

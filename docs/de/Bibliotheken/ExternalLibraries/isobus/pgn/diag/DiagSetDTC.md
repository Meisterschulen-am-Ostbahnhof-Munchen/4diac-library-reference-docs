# DiagSetDTC

![DiagSetDTC](./DiagSetDTC.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Activate or deactivate a diagnostic trouble code (DTC) in the DM1 message

Paket: `isobus::pgn::diag`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Service Initialization
-   **REQ** (Typ: `Event`): Set the DTC (activate or deactivate) (mit Daten: `s16CfHandle`, `u32SPN`, `u8FMI`, `bActive`, `u8NumberOfDM1Messages`)

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialization Confirm
-   **CNF** (Typ: `Event`): Confirmation (mit Daten: `s16Status`)

### **Daten-Eingänge**

-   **s16CfHandle** (Typ: `INT`): Handle of the own (internal) CF (Vorgabewert: `UNVALID`)
-   **u32SPN** (Typ: `UDINT`): Suspect Parameter Number of the trouble code (19 bit)
-   **u8FMI** (Typ: `USINT`): Failure Mode Identifier, see isobus::pgn::diag::FMI
-   **bActive** (Typ: `BOOL`): TRUE = activate, FALSE = deactivate the DTC
-   **u8NumberOfDM1Messages** (Typ: `USINT`): 0 or 255: active until deactivated; 1..254: deactivate automatically after this number of DM1 messages (seconds)

### **Daten-Ausgänge**

-   **s16Status** (Typ: `INT`): Return value of iso_BaseDiagSetDM1DTC(): 0 = OK, < 0 = error

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

Hinweis: Diese Seite wurde aus der Typdefinition (`DiagSetDTC.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

# DLG_Toast

![DLG_Toast](./DLG_Toast.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Hinweis ohne Tasten, verschwindet nach TIMEOUT

Paket: `isobus::UT::dialog`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Initialisierung (mit Daten: `QI`)
-   **SHOW** (Typ: `Event`): Hinweis zeigen (mit Daten: `QI`, `TEXT_ID`, `TITLE_ID`, `ICON`, `TIMEOUT`)
-   **CLOSE** (Typ: `Event`): Hinweis schliessen

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialisierung fertig (mit Daten: `QO`)
-   **CNF** (Typ: `Event`): Hinweis beendet (mit Daten: `QO`)

### **Daten-Eingänge**

-   **QI** (Typ: `BOOL`): Freigabe (Vorgabewert: `TRUE`)
-   **TEXT_ID** (Typ: `UINT`): OutputString Meldungstext (Vorgabewert: `ID_NULL`)
-   **TITLE_ID** (Typ: `UINT`): OutputString Titel, NULL = Standard (Vorgabewert: `ID_NULL`)
-   **ICON** (Typ: `USINT`): DLG_ICON_* (Vorgabewert: `USINT#1`)
-   **TIMEOUT** (Typ: `TIME`): Anzeigedauer (Vorgabewert: `T#3s`)

### **Daten-Ausgänge**

-   **QO** (Typ: `BOOL`): Qualitaet

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

## Funktionsweise (aus der Quelle)

Diese Beschreibung ist aus der Typdefinition abgeleitet und gibt Zustände, Übergänge und Algorithmen wieder. Sie wurde nicht auf Hardware getestet.

### Interne Bausteine

-   **MSG**: Instanz von `isobus::UT::dialog::DLG_MessageBox`

Hinweis: Diese Seite wurde aus der Typdefinition (`DLG_Toast.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

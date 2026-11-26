# DLG_MessageBox

![DLG_MessageBox](./DLG_MessageBox.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: VT-Dialog wie Windows-MessageBox (eine Instanz je Anwendung)

Paket: `isobus::UT::dialog`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Initialisierung (mit Daten: `QI`)
-   **SHOW** (Typ: `Event`): Dialog zeigen (mit Daten: `QI`, `TEXT_ID`, `TITLE_ID`, `ICON`, `BUTTONS`, `DEFAULT_BUTTON`, `MODAL`, `TIMEOUT`, `AUDIO`)
-   **CLOSE** (Typ: `Event`): Dialog schliessen

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialisierung fertig (mit Daten: `QO`)
-   **CNF** (Typ: `Event`): Dialog beendet, RESULT gueltig (mit Daten: `QO`, `RESULT`)

### **Daten-Eingänge**

-   **QI** (Typ: `BOOL`): Freigabe (Vorgabewert: `TRUE`)
-   **TEXT_ID** (Typ: `UINT`): OutputString Meldungstext (Vorgabewert: `ID_NULL`)
-   **TITLE_ID** (Typ: `UINT`): OutputString Titel, NULL = Standard je Icon (Vorgabewert: `ID_NULL`)
-   **ICON** (Typ: `USINT`): DLG_ICON_* (Vorgabewert: `USINT#1`)
-   **BUTTONS** (Typ: `USINT`): DLG_BTN_* (Vorgabewert: `USINT#1`)
-   **DEFAULT_BUTTON** (Typ: `USINT`): Fokus auf Taste 1..3 (Vorgabewert: `USINT#1`)
-   **MODAL** (Typ: `BOOL`): Maske abdecken (Vorgabewert: `FALSE`)
-   **TIMEOUT** (Typ: `TIME`): T#0s = kein Timeout (Vorgabewert: `T#0s`)
-   **AUDIO** (Typ: `BOOL`): Ton beim Oeffnen (Vorgabewert: `FALSE`)

### **Daten-Ausgänge**

-   **QO** (Typ: `BOOL`): Qualitaet
-   **RESULT** (Typ: `USINT`): DLG_RES_*

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

Hinweis: Diese Seite wurde aus der Typdefinition (`DLG_MessageBox.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

# E_MUX_FIFO_2

![E_MUX_FIFO_2](./E_MUX_FIFO_2.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: E_MUX mit Warteschlange: merkt sich bei EI1/EI2 den Kanal (K=0/1)
in einer FIFO und liefert ihn per DATA einzeln aus, den naechsten erst nach
PROCESSED. Gegenstueck ist ein normales E_DEMUX_2 an K.

Paket: `FIFO`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Service Initialization (mit Daten: `QI`, `CAPACITY`, `BUF`)
-   **RST** (Typ: `Event`): Queue leeren und Liefer-Sperre aufheben - Reset der ganzen Kette, der Konsument muss mit zurueckgesetzt werden
-   **EI1** (Typ: `Event`): Kanal 1 (K=0) ans Ende der Queue anhaengen, falls Platz vorhanden
-   **EI2** (Typ: `Event`): Kanal 2 (K=1) ans Ende der Queue anhaengen, falls Platz vorhanden
-   **PROCESSED** (Typ: `Event`): Konsument bestaetigt: der zuletzt per DATA gelieferte Wert ist verarbeitet. Erst danach wird (falls vorhanden) der naechste Wert per DATA geliefert.

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialisierung bestaetigt (mit Daten: `QO`, `BUF`)
-   **RSTO** (Typ: `Event`): Reset bestaetigt (mit Daten: `N`)
-   **PUSHO** (Typ: `Event`): EI1/EI2 bestaetigt (OK=FALSE wenn Queue voll war oder QO=FALSE) (mit Daten: `N`, `OK`)
-   **DATA** (Typ: `Event`): Naechster Kanal in K. Es folgt kein weiteres DATA, bevor nicht PROCESSED eingetroffen ist. (mit Daten: `K`, `N`)

### **Daten-Eingänge**

-   **QI** (Typ: `BOOL`): Event Input Qualifier
-   **CAPACITY** (Typ: `UINT`): Kapazitaet - MUSS der Anzahl Elemente von BUF entsprechen, daher immer ueber F_LEN_ARRAY hinter der PROVIDE_ARR_x_UINT-Instanz ermitteln. Ein groesserer Wert schreibt ueber BUF hinaus. 0 ergibt QO=FALSE.

### **Daten-Ausgänge**

-   **QO** (Typ: `BOOL`): Event Output Qualifier
-   **N** (Typ: `UINT`): Anzahl aktuell wartender (noch nicht per DATA ausgelieferter) Elemente
-   **OK** (Typ: `BOOL`): TRUE wenn das letzte EI1/EI2 erfolgreich war (Queue war nicht voll und QO=TRUE)
-   **K** (Typ: `UINT`): Kanal des zuletzt per DATA gelieferten Ereignisses (0 = EI1, 1 = EI2), gueltig bis PROCESSED - fuer E_DEMUX_2.K

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

Hinweis: Diese Seite wurde aus der Typdefinition (`E_MUX_FIFO_2.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

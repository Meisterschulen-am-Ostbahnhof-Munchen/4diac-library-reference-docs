# FIFO_UINT

![FIFO_UINT](./FIFO_UINT.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: FIFO-Warteschlange fuer UINT-Werte mit frei waehlbarer Kapazitaet und
Verarbeitungs-Handshake: nimmt schnelle Folgen von PUSH entgegen, liefert
aber immer nur EINEN Wert per DATA aus und wartet auf PROCESSED, bevor der
naechste Wert geliefert wird.

Paket: `FIFO`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Service Initialization (mit Daten: `QI`, `CAPACITY`, `BUF`)
-   **RST** (Typ: `Event`): Queue leeren und Liefer-Sperre aufheben - Reset der ganzen Kette, der Konsument muss mit zurueckgesetzt werden
-   **PUSH** (Typ: `Event`): IN ans Ende der Queue anhaengen, falls Platz vorhanden (mit Daten: `IN`)
-   **PROCESSED** (Typ: `Event`): Konsument bestaetigt: der zuletzt per DATA gelieferte Wert ist verarbeitet. Erst danach wird (falls vorhanden) der naechste Wert per DATA geliefert.

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialisierung bestaetigt (mit Daten: `QO`, `BUF`)
-   **RSTO** (Typ: `Event`): Reset bestaetigt (mit Daten: `N`)
-   **PUSHO** (Typ: `Event`): PUSH bestaetigt (OK=FALSE wenn Queue voll war oder QO=FALSE) (mit Daten: `N`, `OK`)
-   **DATA** (Typ: `Event`): Ein neuer Wert ist verfuegbar. Es folgt kein weiteres DATA, bevor nicht PROCESSED eingetroffen ist. (mit Daten: `OUT`, `N`)

### **Daten-Eingänge**

-   **QI** (Typ: `BOOL`): Event Input Qualifier
-   **CAPACITY** (Typ: `UINT`): Kapazitaet - MUSS der Anzahl Elemente von BUF entsprechen, daher immer ueber F_LEN_ARRAY hinter der PROVIDE_ARR_x_UINT-Instanz ermitteln. Ein groesserer Wert schreibt ueber BUF hinaus. 0 ergibt QO=FALSE.
-   **IN** (Typ: `UINT`): Zu pushender Wert

### **Daten-Ausgänge**

-   **QO** (Typ: `BOOL`): Event Output Qualifier
-   **N** (Typ: `UINT`): Anzahl aktuell wartender (noch nicht per DATA ausgelieferter) Elemente
-   **OK** (Typ: `BOOL`): TRUE wenn der letzte PUSH erfolgreich war (Queue war nicht voll und QO=TRUE)
-   **OUT** (Typ: `UINT`): Zuletzt per DATA gelieferter Wert, gueltig bis zur naechsten PROCESSED-Bestaetigung

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

Hinweis: Diese Seite wurde aus der Typdefinition (`FIFO_UINT.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

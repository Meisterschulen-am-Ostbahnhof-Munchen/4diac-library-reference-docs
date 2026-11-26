# FIFO_UINT_Adapter

![FIFO_UINT_Adapter](./FIFO_UINT_Adapter.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: FIFO-Warteschlange fuer UINT-Werte mit frei waehlbarer Kapazitaet - wie
FIFO::FIFO_UINT, aber OHNE eigene DATA/PROCESSED-Events auf der
Konsumentenseite: der Konsument haengt stattdessen per Socket am Plug
OUT_DATA (Typ AFIFO_UINT) an. Die Verarbeitungs-Quittung laeuft ueber den
Rueckkanal des Adapters selbst (PROCESSED), kein separater Wrapper noetig.

Paket: `FIFO_adapter`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Service Initialization (mit Daten: `QI`, `CAPACITY`, `BUF`)
-   **RST** (Typ: `Event`): Queue leeren und Liefer-Sperre aufheben - Reset der ganzen Kette, der Konsument muss mit zurueckgesetzt werden
-   **PUSH** (Typ: `Event`): IN ans Ende der Queue anhaengen, falls Platz vorhanden (mit Daten: `IN`)

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialisierung bestaetigt (mit Daten: `QO`, `BUF`)
-   **RSTO** (Typ: `Event`): Reset bestaetigt (mit Daten: `N`)
-   **PUSHO** (Typ: `Event`): PUSH bestaetigt (OK=FALSE wenn Queue voll war oder QO=FALSE) (mit Daten: `N`, `OK`)

### **Daten-Eingänge**

-   **QI** (Typ: `BOOL`): Event Input Qualifier
-   **CAPACITY** (Typ: `UINT`): Kapazitaet - MUSS der Anzahl Elemente von BUF entsprechen, daher immer ueber F_LEN_ARRAY hinter der PROVIDE_ARR_x_UINT-Instanz ermitteln. Ein groesserer Wert schreibt ueber BUF hinaus. 0 ergibt QO=FALSE.
-   **IN** (Typ: `UINT`): Zu pushender Wert

### **Daten-Ausgänge**

-   **QO** (Typ: `BOOL`): Event Output Qualifier
-   **N** (Typ: `UINT`): Anzahl aktuell wartender (noch nicht ausgelieferter) Elemente
-   **OK** (Typ: `BOOL`): TRUE wenn der letzte PUSH erfolgreich war (Queue war nicht voll und QO=TRUE)

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

-   **OUT_DATA** (Typ: `FIFO_adapter::AFIFO_UINT`): Konsument haengt per Socket hier an - DATA liefert den naechsten Wert, PROCESSED (Rueckkanal) quittiert

Hinweis: Diese Seite wurde aus der Typdefinition (`FIFO_UINT_Adapter.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

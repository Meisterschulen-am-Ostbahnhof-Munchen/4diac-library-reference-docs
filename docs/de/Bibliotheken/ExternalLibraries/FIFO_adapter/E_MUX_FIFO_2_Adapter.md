# E_MUX_FIFO_2_Adapter

![E_MUX_FIFO_2_Adapter](./E_MUX_FIFO_2_Adapter.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Adapter-Variante von FIFO::E_MUX_FIFO_2: merkt sich bei EI1/EI2 den Kanal
(0/1) in einer FIFO und liefert ihn ueber den Plug OUT_DATA (AFIFO_UINT, Kanal in
OUT) einzeln aus. PROCESSED kommt ueber den Rueckkanal. Gegenstueck: E_DEMUX_FIFO_2.

Paket: `FIFO_adapter`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Service Initialization (mit Daten: `QI`, `CAPACITY`, `BUF`)
-   **RST** (Typ: `Event`): Queue leeren und Liefer-Sperre aufheben - Reset der ganzen Kette, der Konsument muss mit zurueckgesetzt werden
-   **EI1** (Typ: `Event`): Kanal 1 (0) ans Ende der Queue anhaengen, falls Platz vorhanden
-   **EI2** (Typ: `Event`): Kanal 2 (1) ans Ende der Queue anhaengen, falls Platz vorhanden

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialisierung bestaetigt (mit Daten: `QO`, `BUF`)
-   **RSTO** (Typ: `Event`): Reset bestaetigt (mit Daten: `N`)
-   **PUSHO** (Typ: `Event`): EI1/EI2 bestaetigt (OK=FALSE wenn Queue voll war oder QO=FALSE) (mit Daten: `N`, `OK`)

### **Daten-Eingänge**

-   **QI** (Typ: `BOOL`): Event Input Qualifier
-   **CAPACITY** (Typ: `UINT`): Kapazitaet - MUSS der Anzahl Elemente von BUF entsprechen, daher immer ueber F_LEN_ARRAY hinter der PROVIDE_ARR_x_UINT-Instanz ermitteln. Ein groesserer Wert schreibt ueber BUF hinaus. 0 ergibt QO=FALSE.

### **Daten-Ausgänge**

-   **QO** (Typ: `BOOL`): Event Output Qualifier
-   **N** (Typ: `UINT`): Anzahl aktuell wartender (noch nicht ausgelieferter) Elemente
-   **OK** (Typ: `BOOL`): TRUE wenn das letzte EI1/EI2 erfolgreich war (Queue war nicht voll und QO=TRUE)

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

-   **OUT_DATA** (Typ: `FIFO_adapter::AFIFO_UINT`): Konsument haengt per Socket hier an - DATA liefert den naechsten Wert, PROCESSED (Rueckkanal) quittiert

Hinweis: Diese Seite wurde aus der Typdefinition (`E_MUX_FIFO_2_Adapter.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

# E_DEMUX_FIFO_2

![E_DEMUX_FIFO_2](./E_DEMUX_FIFO_2.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Gegenstueck zu E_MUX_FIFO_2_Adapter: verteilt DATA vom Socket IN_DATA nach dem
Kanal (OUT = 0/1) auf EO1/EO2. PROCESSED vom Konsumenten geht ueber den Rueckkanal
des Adapters zurueck, erst dann folgt das naechste Ereignis.

Paket: `FIFO_adapter`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **PROCESSED** (Typ: `Event`): Konsument bestaetigt: das zuletzt ausgegebene Ereignis ist verarbeitet

### **Ereignis-Ausgänge**

-   **EO1** (Typ: `Event`): Ereignis von Kanal 1 (K = 0) (mit Daten: `K`, `N`)
-   **EO2** (Typ: `Event`): Ereignis von Kanal 2 (K = 1) (mit Daten: `K`, `N`)

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

-   **K** (Typ: `UINT`): Kanal des ausgegebenen Ereignisses
-   **N** (Typ: `UINT`): Anzahl der danach noch wartenden Ereignisse in der Queue

### **Adapter (Sockets)**

-   **IN_DATA** (Typ: `FIFO_adapter::AFIFO_UINT`): Hier haengt der Plug OUT_DATA von E_MUX_FIFO_2_Adapter an

### **Adapter (Plugs)**

Keine.

Hinweis: Diese Seite wurde aus der Typdefinition (`E_DEMUX_FIFO_2.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

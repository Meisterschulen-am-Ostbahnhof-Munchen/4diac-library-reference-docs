# E_DEMUX_3

![E_DEMUX_3](./E_DEMUX_3.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Event demultiplexer (generic FB)

Paket: `iec61499::events`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **EI** (Typ: `Event`): Event to demultiplex (mit Daten: `K`)

### **Ereignis-Ausgänge**

-   **EO1** (Typ: `Event`): Output, demultiplexed from EI when K=0
-   **EO2** (Typ: `Event`): Output, demultiplexed from EI when K=1
-   **EO3** (Typ: `Event`): Output, demultiplexed from EI when K=2

### **Daten-Eingänge**

-   **K** (Typ: `UINT`): Event index

### **Daten-Ausgänge**

Keine.

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

Hinweis: Diese Seite wurde aus der Typdefinition (`E_DEMUX_3.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

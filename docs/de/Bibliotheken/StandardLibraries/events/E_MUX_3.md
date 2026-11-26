# E_MUX_3

![E_MUX_3](./E_MUX_3.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Event multiplexer (generic FB)

Paket: `iec61499::events`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **EI1** (Typ: `Event`): Event to multiplex, K=0
-   **EI2** (Typ: `Event`): Event to multiplex, K=1
-   **EI3** (Typ: `Event`): Event to multiplex, K=2

### **Ereignis-Ausgänge**

-   **EO** (Typ: `Event`): Event which was multiplexed (mit Daten: `K`)

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

-   **K** (Typ: `UINT`): Event index

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

Hinweis: Diese Seite wurde aus der Typdefinition (`E_MUX_3.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

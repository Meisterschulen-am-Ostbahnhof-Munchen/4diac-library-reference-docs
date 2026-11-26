# E_MUX_5

![E_MUX_5](./E_MUX_5.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Event multiplexer (generic FB)

Paket: `iec61499::events`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **EI1** (Typ: `Event`): Event to multiplex, K=0
-   **EI2** (Typ: `Event`): Event to multiplex, K=1
-   **EI3** (Typ: `Event`): Event to multiplex, K=2
-   **EI4** (Typ: `Event`): Event to multiplex, K=3
-   **EI5** (Typ: `Event`): Event to multiplex, K=4

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

Hinweis: Diese Seite wurde aus der Typdefinition (`E_MUX_5.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

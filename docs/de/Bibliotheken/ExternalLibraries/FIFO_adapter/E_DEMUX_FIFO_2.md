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

## Funktionsweise (aus der Quelle)

Diese Beschreibung ist aus der Typdefinition abgeleitet und gibt Zustände, Übergänge und Algorithmen wieder. Sie wurde nicht auf Hardware getestet.

### Ablaufsteuerung (ECC)

-   **START** (Aktion: keine Aktion)
-   **S_DATA** (Aktion: `COPY`)
-   **S_EO1** (Aktion: sendet Ereignis `EO1`)
-   **S_EO2** (Aktion: sendet Ereignis `EO2`)
-   **S_SKIP** (Aktion: sendet Ereignis `IN_DATA.PROCESSED`)
-   **S_PROCESSED** (Aktion: sendet Ereignis `IN_DATA.PROCESSED`)

**Übergänge**

-   START → S_DATA, wenn `IN_DATA.DATA`
-   S_DATA → S_EO1, wenn `K = 0`
-   S_DATA → S_EO2, wenn `K = 1`
-   S_DATA → S_SKIP, wenn `1`
-   S_EO1 → START, wenn `1`
-   S_EO2 → START, wenn `1`
-   S_SKIP → START, wenn `1`
-   START → S_PROCESSED, wenn `PROCESSED`
-   S_PROCESSED → START, wenn `1`

### Algorithmen

**COPY**: Kanal und Fuellstand vom Adapter uebernehmen

```iecst
K := IN_DATA.OUT;
N := IN_DATA.N;
```

Hinweis: Diese Seite wurde aus der Typdefinition (`E_DEMUX_FIFO_2.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

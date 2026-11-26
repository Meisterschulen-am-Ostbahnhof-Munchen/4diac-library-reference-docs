# AFIFO_UINT_TO_E

![AFIFO_UINT_TO_E](./AFIFO_UINT_TO_E.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Konverter: Socket AFIFO_UINT auf normale Events. DATA mit OUT/N kommt vom
FIFO, PROCESSED geht ueber den Rueckkanal des Adapters an den FIFO zurueck.

Paket: `FIFO_adapter`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **PROCESSED** (Typ: `Event`): Konsument bestaetigt: der zuletzt per DATA gelieferte Wert ist verarbeitet

### **Ereignis-Ausgänge**

-   **DATA** (Typ: `Event`): Neuer Wert vom FIFO - es folgt kein weiteres DATA, bevor PROCESSED eingetroffen ist (mit Daten: `OUT`, `N`)

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

-   **OUT** (Typ: `UINT`): Gelieferter Wert
-   **N** (Typ: `UINT`): Anzahl der danach noch wartenden Elemente in der Queue

### **Adapter (Sockets)**

-   **IN_DATA** (Typ: `FIFO_adapter::AFIFO_UINT`): Hier haengt der Plug OUT_DATA von FIFO_UINT_Adapter an

### **Adapter (Plugs)**

Keine.

## Funktionsweise (aus der Quelle)

Diese Beschreibung ist aus der Typdefinition abgeleitet und gibt Zustände, Übergänge und Algorithmen wieder. Sie wurde nicht auf Hardware getestet.

### Ablaufsteuerung (ECC)

-   **START** (Aktion: keine Aktion)
-   **S_DATA** (Aktion: `COPY` → sendet Ereignis `DATA`)
-   **S_PROCESSED** (Aktion: sendet Ereignis `IN_DATA.PROCESSED`)

**Übergänge**

-   START → S_DATA, wenn `IN_DATA.DATA`
-   S_DATA → START, wenn `1`
-   START → S_PROCESSED, wenn `PROCESSED`
-   S_PROCESSED → START, wenn `1`

### Algorithmen

**COPY**: Wert und Fuellstand vom Adapter uebernehmen

```iecst
OUT := IN_DATA.OUT;
N := IN_DATA.N;
```

Hinweis: Diese Seite wurde aus der Typdefinition (`AFIFO_UINT_TO_E.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

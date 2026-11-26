# AFIFO_UINT_TO_E

![AFIFO_UINT_TO_E](./AFIFO_UINT_TO_E.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Konverter: Socket AFIFO_UINT auf normale Events. DATA mit OUT/N kommt vom
FIFO, PROCESSED geht ueber den Rueckkanal des Adapters an den FIFO zurueck.

Package: `FIFO_adapter`

## Interface structure

### **Event inputs**

-   **PROCESSED** (Type: `Event`): Konsument bestaetigt: der zuletzt per DATA gelieferte Wert ist verarbeitet

### **Event outputs**

-   **DATA** (Type: `Event`): Neuer Wert vom FIFO - es folgt kein weiteres DATA, bevor PROCESSED eingetroffen ist (with data: `OUT`, `N`)

### **Data inputs**

None.

### **Data outputs**

-   **OUT** (Type: `UINT`): Gelieferter Wert
-   **N** (Type: `UINT`): Anzahl der danach noch wartenden Elemente in der Queue

### **Adapters (sockets)**

-   **IN_DATA** (Type: `FIFO_adapter::AFIFO_UINT`): Hier haengt der Plug OUT_DATA von FIFO_UINT_Adapter an

### **Adapters (plugs)**

None.

## Operation (from the source)

This description is derived from the type definition and shows states, transitions and algorithms. It has not been tested on hardware.

### Execution control chart (ECC)

-   **START** (Action: no action)
-   **S_DATA** (Action: `COPY` → sends event `DATA`)
-   **S_PROCESSED** (Action: sends event `IN_DATA.PROCESSED`)

**Transitions**

-   START → S_DATA, when `IN_DATA.DATA`
-   S_DATA → START, when `1`
-   START → S_PROCESSED, when `PROCESSED`
-   S_PROCESSED → START, when `1`

### Algorithms

**COPY**: Wert und Fuellstand vom Adapter uebernehmen

```iecst
OUT := IN_DATA.OUT;
N := IN_DATA.N;
```

Note: This page was generated from the type definition (`AFIFO_UINT_TO_E.fbt`) and only shows the interface. A detailed description of the behavior is still missing.

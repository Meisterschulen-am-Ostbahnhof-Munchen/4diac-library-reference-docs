# E_DEMUX_FIFO_2

![E_DEMUX_FIFO_2](./E_DEMUX_FIFO_2.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Gegenstueck zu E_MUX_FIFO_2_Adapter: verteilt DATA vom Socket IN_DATA nach dem
Kanal (OUT = 0/1) auf EO1/EO2. PROCESSED vom Konsumenten geht ueber den Rueckkanal
des Adapters zurueck, erst dann folgt das naechste Ereignis.

Package: `FIFO_adapter`

## Interface structure

### **Event inputs**

-   **PROCESSED** (Type: `Event`): Konsument bestaetigt: das zuletzt ausgegebene Ereignis ist verarbeitet

### **Event outputs**

-   **EO1** (Type: `Event`): Ereignis von Kanal 1 (K = 0) (with data: `K`, `N`)
-   **EO2** (Type: `Event`): Ereignis von Kanal 2 (K = 1) (with data: `K`, `N`)

### **Data inputs**

None.

### **Data outputs**

-   **K** (Type: `UINT`): Kanal des ausgegebenen Ereignisses
-   **N** (Type: `UINT`): Anzahl der danach noch wartenden Ereignisse in der Queue

### **Adapters (sockets)**

-   **IN_DATA** (Type: `FIFO_adapter::AFIFO_UINT`): Hier haengt der Plug OUT_DATA von E_MUX_FIFO_2_Adapter an

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`E_DEMUX_FIFO_2.fbt`) and only shows the interface. A detailed description of the behavior is still missing.

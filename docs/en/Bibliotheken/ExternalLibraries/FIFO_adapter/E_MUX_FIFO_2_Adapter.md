# E_MUX_FIFO_2_Adapter

![E_MUX_FIFO_2_Adapter](./E_MUX_FIFO_2_Adapter.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Adapter-Variante von FIFO::E_MUX_FIFO_2: merkt sich bei EI1/EI2 den Kanal
(0/1) in einer FIFO und liefert ihn ueber den Plug OUT_DATA (AFIFO_UINT, Kanal in
OUT) einzeln aus. PROCESSED kommt ueber den Rueckkanal. Gegenstueck: E_DEMUX_FIFO_2.

Package: `FIFO_adapter`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Service Initialization (with data: `QI`, `CAPACITY`, `BUF`)
-   **RST** (Type: `Event`): Queue leeren und Liefer-Sperre aufheben - Reset der ganzen Kette, der Konsument muss mit zurueckgesetzt werden
-   **EI1** (Type: `Event`): Kanal 1 (0) ans Ende der Queue anhaengen, falls Platz vorhanden
-   **EI2** (Type: `Event`): Kanal 2 (1) ans Ende der Queue anhaengen, falls Platz vorhanden

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialisierung bestaetigt (with data: `QO`, `BUF`)
-   **RSTO** (Type: `Event`): Reset bestaetigt (with data: `N`)
-   **PUSHO** (Type: `Event`): EI1/EI2 bestaetigt (OK=FALSE wenn Queue voll war oder QO=FALSE) (with data: `N`, `OK`)

### **Data inputs**

-   **QI** (Type: `BOOL`): Event Input Qualifier
-   **CAPACITY** (Type: `UINT`): Kapazitaet - MUSS der Anzahl Elemente von BUF entsprechen, daher immer ueber F_LEN_ARRAY hinter der PROVIDE_ARR_x_UINT-Instanz ermitteln. Ein groesserer Wert schreibt ueber BUF hinaus. 0 ergibt QO=FALSE.

### **Data outputs**

-   **QO** (Type: `BOOL`): Event Output Qualifier
-   **N** (Type: `UINT`): Anzahl aktuell wartender (noch nicht ausgelieferter) Elemente
-   **OK** (Type: `BOOL`): TRUE wenn das letzte EI1/EI2 erfolgreich war (Queue war nicht voll und QO=TRUE)

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

-   **OUT_DATA** (Type: `FIFO_adapter::AFIFO_UINT`): Konsument haengt per Socket hier an - DATA liefert den naechsten Wert, PROCESSED (Rueckkanal) quittiert

Note: This page was generated from the type definition (`E_MUX_FIFO_2_Adapter.fbt`) and only shows the interface. A detailed description of the behavior is still missing.

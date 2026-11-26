# E_MUX_FIFO_2

![E_MUX_FIFO_2](./E_MUX_FIFO_2.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: E_MUX mit Warteschlange: merkt sich bei EI1/EI2 den Kanal (K=0/1)
in einer FIFO und liefert ihn per DATA einzeln aus, den naechsten erst nach
PROCESSED. Gegenstueck ist ein normales E_DEMUX_2 an K.

Package: `FIFO`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Service Initialization (with data: `QI`, `CAPACITY`, `BUF`)
-   **RST** (Type: `Event`): Queue leeren und Liefer-Sperre aufheben - Reset der ganzen Kette, der Konsument muss mit zurueckgesetzt werden
-   **EI1** (Type: `Event`): Kanal 1 (K=0) ans Ende der Queue anhaengen, falls Platz vorhanden
-   **EI2** (Type: `Event`): Kanal 2 (K=1) ans Ende der Queue anhaengen, falls Platz vorhanden
-   **PROCESSED** (Type: `Event`): Konsument bestaetigt: der zuletzt per DATA gelieferte Wert ist verarbeitet. Erst danach wird (falls vorhanden) der naechste Wert per DATA geliefert.

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialisierung bestaetigt (with data: `QO`, `BUF`)
-   **RSTO** (Type: `Event`): Reset bestaetigt (with data: `N`)
-   **PUSHO** (Type: `Event`): EI1/EI2 bestaetigt (OK=FALSE wenn Queue voll war oder QO=FALSE) (with data: `N`, `OK`)
-   **DATA** (Type: `Event`): Naechster Kanal in K. Es folgt kein weiteres DATA, bevor nicht PROCESSED eingetroffen ist. (with data: `K`, `N`)

### **Data inputs**

-   **QI** (Type: `BOOL`): Event Input Qualifier
-   **CAPACITY** (Type: `UINT`): Kapazitaet - MUSS der Anzahl Elemente von BUF entsprechen, daher immer ueber F_LEN_ARRAY hinter der PROVIDE_ARR_x_UINT-Instanz ermitteln. Ein groesserer Wert schreibt ueber BUF hinaus. 0 ergibt QO=FALSE.

### **Data outputs**

-   **QO** (Type: `BOOL`): Event Output Qualifier
-   **N** (Type: `UINT`): Anzahl aktuell wartender (noch nicht per DATA ausgelieferter) Elemente
-   **OK** (Type: `BOOL`): TRUE wenn das letzte EI1/EI2 erfolgreich war (Queue war nicht voll und QO=TRUE)
-   **K** (Type: `UINT`): Kanal des zuletzt per DATA gelieferten Ereignisses (0 = EI1, 1 = EI2), gueltig bis PROCESSED - fuer E_DEMUX_2.K

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`E_MUX_FIFO_2.fbt`) and only shows the interface. A detailed description of the behavior is still missing.

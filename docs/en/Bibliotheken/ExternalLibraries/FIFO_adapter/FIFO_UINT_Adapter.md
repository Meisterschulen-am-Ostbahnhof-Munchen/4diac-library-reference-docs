# FIFO_UINT_Adapter

![FIFO_UINT_Adapter](./FIFO_UINT_Adapter.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: FIFO-Warteschlange fuer UINT-Werte mit frei waehlbarer Kapazitaet - wie
FIFO::FIFO_UINT, aber OHNE eigene DATA/PROCESSED-Events auf der
Konsumentenseite: der Konsument haengt stattdessen per Socket am Plug
OUT_DATA (Typ AFIFO_UINT) an. Die Verarbeitungs-Quittung laeuft ueber den
Rueckkanal des Adapters selbst (PROCESSED), kein separater Wrapper noetig.

Package: `FIFO_adapter`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Service Initialization (with data: `QI`, `CAPACITY`, `BUF`)
-   **RST** (Type: `Event`): Queue leeren und Liefer-Sperre aufheben - Reset der ganzen Kette, der Konsument muss mit zurueckgesetzt werden
-   **PUSH** (Type: `Event`): IN ans Ende der Queue anhaengen, falls Platz vorhanden (with data: `IN`)

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialisierung bestaetigt (with data: `QO`, `BUF`)
-   **RSTO** (Type: `Event`): Reset bestaetigt (with data: `N`)
-   **PUSHO** (Type: `Event`): PUSH bestaetigt (OK=FALSE wenn Queue voll war oder QO=FALSE) (with data: `N`, `OK`)

### **Data inputs**

-   **QI** (Type: `BOOL`): Event Input Qualifier
-   **CAPACITY** (Type: `UINT`): Kapazitaet - MUSS der Anzahl Elemente von BUF entsprechen, daher immer ueber F_LEN_ARRAY hinter der PROVIDE_ARR_x_UINT-Instanz ermitteln. Ein groesserer Wert schreibt ueber BUF hinaus. 0 ergibt QO=FALSE.
-   **IN** (Type: `UINT`): Zu pushender Wert

### **Data outputs**

-   **QO** (Type: `BOOL`): Event Output Qualifier
-   **N** (Type: `UINT`): Anzahl aktuell wartender (noch nicht ausgelieferter) Elemente
-   **OK** (Type: `BOOL`): TRUE wenn der letzte PUSH erfolgreich war (Queue war nicht voll und QO=TRUE)

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

-   **OUT_DATA** (Type: `FIFO_adapter::AFIFO_UINT`): Konsument haengt per Socket hier an - DATA liefert den naechsten Wert, PROCESSED (Rueckkanal) quittiert

Note: This page was generated from the type definition (`FIFO_UINT_Adapter.fbt`) and only shows the interface. A detailed description of the behavior is still missing.

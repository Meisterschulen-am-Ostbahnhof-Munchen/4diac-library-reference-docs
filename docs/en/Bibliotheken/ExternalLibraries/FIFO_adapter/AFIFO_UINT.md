# AFIFO_UINT

![AFIFO_UINT](./AFIFO_UINT.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Adapter fuer FIFO_UINT (FIFO_adapter): Konsument haengt per Socket an - DATA
liefert PLUG->SOCKET den naechsten Wert, PROCESSED quittiert SOCKET->PLUG
dessen Verarbeitung. Kein eigener Event-Baustein auf Konsumentenseite noetig -
der Rueckkanal des Adapters selbst traegt die Quittung.

Package: `FIFO_adapter`

## Interface structure

### **Event inputs**

-   **PROCESSED** (Type: `Event`): Konsument (SOCKET) bestaetigt: OUT ist verarbeitet, der naechste Wert (falls vorhanden) kann geliefert werden

### **Event outputs**

-   **DATA** (Type: `Event`): PLUG (FIFO) liefert einen neuen Wert - es folgt kein weiteres DATA, bevor nicht PROCESSED eingetroffen ist (with data: `OUT`, `N`)

### **Data inputs**

None.

### **Data outputs**

-   **OUT** (Type: `UINT`): Gelieferter Wert, gueltig bis zur naechsten PROCESSED-Bestaetigung
-   **N** (Type: `UINT`): Anzahl der danach noch wartenden Elemente in der Queue

Note: This page was generated from the type definition (`AFIFO_UINT.adp`) and only shows the interface. A detailed description of the behavior is still missing.

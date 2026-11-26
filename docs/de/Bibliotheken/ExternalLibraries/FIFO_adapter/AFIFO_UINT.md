# AFIFO_UINT

![AFIFO_UINT](./AFIFO_UINT.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Adapter fuer FIFO_UINT (FIFO_adapter): Konsument haengt per Socket an - DATA
liefert PLUG->SOCKET den naechsten Wert, PROCESSED quittiert SOCKET->PLUG
dessen Verarbeitung. Kein eigener Event-Baustein auf Konsumentenseite noetig -
der Rueckkanal des Adapters selbst traegt die Quittung.

Paket: `FIFO_adapter`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **PROCESSED** (Typ: `Event`): Konsument (SOCKET) bestaetigt: OUT ist verarbeitet, der naechste Wert (falls vorhanden) kann geliefert werden

### **Ereignis-Ausgänge**

-   **DATA** (Typ: `Event`): PLUG (FIFO) liefert einen neuen Wert - es folgt kein weiteres DATA, bevor nicht PROCESSED eingetroffen ist (mit Daten: `OUT`, `N`)

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

-   **OUT** (Typ: `UINT`): Gelieferter Wert, gueltig bis zur naechsten PROCESSED-Bestaetigung
-   **N** (Typ: `UINT`): Anzahl der danach noch wartenden Elemente in der Queue

Hinweis: Diese Seite wurde aus der Typdefinition (`AFIFO_UINT.adp`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.

# A2X_AND_4_UNGATED

![A2X_AND_4_UNGATED](A2X_AND_4_UNGATED.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`A2X_AND_4_UNGATED` verknüpft 4 [A2X](../types/unidirectional/BOOL/A2X.md)-Adapter mit UND, **UP und DOWN getrennt**. Der Baustein ist generisch (`GEN_A2X_AND`), die Zahl der Eingänge steckt im Namen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine direkten Ereignisse.

### **Ereignis-Ausgänge**

Keine direkten Ereignisse.

### **Daten-Eingänge**

Keine direkten Daten.

### **Daten-Ausgänge**

Keine direkten Daten.

### **Adapter**

| Rolle | Name | Typ | Beschreibung |
| --- | --- | --- | --- |
| Socket | IN1 | adapter::types::unidirectional::A2X | AND input 1 |
| Socket | IN2 | adapter::types::unidirectional::A2X | AND input 2 |
| Socket | IN3 | adapter::types::unidirectional::A2X | AND input 3 |
| Socket | IN4 | adapter::types::unidirectional::A2X | AND input 4 |
| Plug | OUT | adapter::types::unidirectional::A2X | AND result |

## Funktionsweise

Die Variante `_UNGATED` hat **keine Änderungserkennung**: Jedes neu berechnete Ergebnis geht bedingungslos an den Ausgang `OUT`. Das ist für Verbraucher gedacht, die eine periodische Kadenz unabhängig von Wertänderungen brauchen, zum Beispiel Ableitungs- oder Frequenzberechnungen.

## Technische Besonderheiten

- Generischer Baustein (`GenericClassName` = `GEN_A2X_AND`), keine eigene Netzwerkdefinition im Typ.
- UP und DOWN werden getrennt verknüpft.
- Die Implementierung liegt in der Firmware (C++), nicht im Typ.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- Gleicher Baustein mit anderer Verknüpfung: [A2X_OR_4_UNGATED](A2X_OR_4_UNGATED.md), [A2X_XOR_4_UNGATED](A2X_XOR_4_UNGATED.md).
- Gleiche Verknüpfung mit anderer Eingangszahl: [A2X_AND_2_UNGATED](A2X_AND_2_UNGATED.md), [A2X_AND_3_UNGATED](A2X_AND_3_UNGATED.md).
- Einkanaliger Vorgänger: [AX_AND_4_UNGATED](AX_AND_4_UNGATED.md).

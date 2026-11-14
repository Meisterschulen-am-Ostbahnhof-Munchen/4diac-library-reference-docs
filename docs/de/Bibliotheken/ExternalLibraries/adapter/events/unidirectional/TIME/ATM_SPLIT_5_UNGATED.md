# ATM_SPLIT_5_UNGATED

![ATM_SPLIT_5_UNGATED](ATM_SPLIT_5_UNGATED.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`ATM_SPLIT_5_UNGATED` verteilt **einen** ATM-Adapter auf 5 gleiche Adapter-Ausgänge. Der Baustein ist generisch (`GEN_ATM_SPLIT`).

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
| Socket | IN | adapter::types::unidirectional::ATM |  |
| Plug | OUT1 | adapter::types::unidirectional::ATM |  |
| Plug | OUT2 | adapter::types::unidirectional::ATM |  |
| Plug | OUT3 | adapter::types::unidirectional::ATM |  |
| Plug | OUT4 | adapter::types::unidirectional::ATM |  |
| Plug | OUT5 | adapter::types::unidirectional::ATM |  |

## Funktionsweise

Die Variante `_UNGATED` hat **keine Änderungserkennung**: Jedes eingehende Ereignis geht bedingungslos an alle 5 Ausgänge. Das ist für Verbraucher gedacht, die eine periodische Kadenz unabhängig von Wertänderungen brauchen, zum Beispiel Ableitungs- oder Frequenzberechnungen.

## Technische Besonderheiten

- Generischer Baustein (`GenericClassName` = `GEN_ATM_SPLIT`), keine eigene Netzwerkdefinition im Typ.
- Die Implementierung liegt in der Firmware (C++), nicht im Typ.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- Andere Ausgangszahl: [ATM_SPLIT_6_UNGATED](ATM_SPLIT_6_UNGATED.md), [ATM_SPLIT_7_UNGATED](ATM_SPLIT_7_UNGATED.md), [ATM_SPLIT_8_UNGATED](ATM_SPLIT_8_UNGATED.md), [ATM_SPLIT_9_UNGATED](ATM_SPLIT_9_UNGATED.md).
- Mit Änderungserkennung bzw. ohne: [ATM_SPLIT_5](ATM_SPLIT_5.md).

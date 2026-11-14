# A2X_OR_3

![A2X_OR_3](A2X_OR_3.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`A2X_OR_3` verknüpft 3 [A2X](../types/unidirectional/BOOL/A2X.md)-Adapter mit ODER, **UP und DOWN getrennt**. Der Baustein ist generisch (`GEN_A2X_OR`), die Zahl der Eingänge steckt im Namen.

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
| Socket | IN1 | adapter::types::unidirectional::A2X | OR input 1 |
| Socket | IN2 | adapter::types::unidirectional::A2X | OR input 2 |
| Socket | IN3 | adapter::types::unidirectional::A2X | OR input 3 |
| Plug | OUT | adapter::types::unidirectional::A2X | OR result |

## Funktionsweise

Der Adapter-Ausgang `OUT` wird **nur bei einer Wertänderung** aktualisiert, das Ereignis kommt nur bei tatsächlicher Änderung. Für die Variante ohne Änderungserkennung gibt es `A2X_OR_3_UNGATED`.

## Technische Besonderheiten

- Generischer Baustein (`GenericClassName` = `GEN_A2X_OR`), keine eigene Netzwerkdefinition im Typ.
- UP und DOWN werden getrennt verknüpft.
- Die Implementierung liegt in der Firmware (C++), nicht im Typ.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- Gleicher Baustein mit anderer Verknüpfung: [A2X_AND_3](A2X_AND_3.md), [A2X_XOR_3](A2X_XOR_3.md).
- Gleiche Verknüpfung mit anderer Eingangszahl: [A2X_OR_2](A2X_OR_2.md), [A2X_OR_4](A2X_OR_4.md), [A2X_OR_5](A2X_OR_5.md), [A2X_OR_6](A2X_OR_6.md), [A2X_OR_7](A2X_OR_7.md), [A2X_OR_8](A2X_OR_8.md), [A2X_OR_9](A2X_OR_9.md), [A2X_OR_10](A2X_OR_10.md).
- Einkanaliger Vorgänger: [AX_OR_3](AX_OR_3.md).

# A2X_XOR_6

![A2X_XOR_6](A2X_XOR_6.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`A2X_XOR_6` verknüpft 6 [A2X](../types/unidirectional/BOOL/A2X.md)-Adapter mit Exklusiv-ODER, **UP und DOWN getrennt**. Der Baustein ist generisch (`GEN_A2X_XOR`), die Zahl der Eingänge steckt im Namen.

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
| Socket | IN1 | adapter::types::unidirectional::A2X | XOR input 1 |
| Socket | IN2 | adapter::types::unidirectional::A2X | XOR input 2 |
| Socket | IN3 | adapter::types::unidirectional::A2X | XOR input 3 |
| Socket | IN4 | adapter::types::unidirectional::A2X | XOR input 4 |
| Socket | IN5 | adapter::types::unidirectional::A2X | XOR input 5 |
| Socket | IN6 | adapter::types::unidirectional::A2X | XOR input 6 |
| Plug | OUT | adapter::types::unidirectional::A2X | XOR result |

## Funktionsweise

Der Adapter-Ausgang `OUT` wird **nur bei einer Wertänderung** aktualisiert, das Ereignis kommt nur bei tatsächlicher Änderung. Für die Variante ohne Änderungserkennung gibt es `A2X_XOR_6_UNGATED`.

## Technische Besonderheiten

- Generischer Baustein (`GenericClassName` = `GEN_A2X_XOR`), keine eigene Netzwerkdefinition im Typ.
- UP und DOWN werden getrennt verknüpft.
- Die Implementierung liegt in der Firmware (C++), nicht im Typ.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- Gleicher Baustein mit anderer Verknüpfung: [A2X_AND_6](A2X_AND_6.md), [A2X_OR_6](A2X_OR_6.md).
- Gleiche Verknüpfung mit anderer Eingangszahl: [A2X_XOR_2](A2X_XOR_2.md), [A2X_XOR_3](A2X_XOR_3.md), [A2X_XOR_4](A2X_XOR_4.md), [A2X_XOR_5](A2X_XOR_5.md), [A2X_XOR_7](A2X_XOR_7.md), [A2X_XOR_8](A2X_XOR_8.md).
- Einkanaliger Vorgänger: [AX_XOR_6](AX_XOR_6.md).

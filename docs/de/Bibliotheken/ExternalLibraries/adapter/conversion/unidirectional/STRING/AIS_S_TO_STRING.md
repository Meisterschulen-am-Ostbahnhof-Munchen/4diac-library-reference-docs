# AIS_S_TO_STRING

![AIS_S_TO_STRING](AIS_S_TO_STRING.svg)

![AIS_S_TO_STRING_network](./AIS_S_TO_STRING_network.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`AIS_S_TO_STRING` ist ein Composite-Baustein: Composite FB for convert AIS to STRING.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine direkten Ereignisse.

### **Ereignis-Ausgänge**

| Ereignis | Typ | Mit | Beschreibung |
| --- | --- | --- | --- |
| CNF | Event | IN | Confirmation from Adapter |

### **Daten-Eingänge**

Keine direkten Daten.

### **Daten-Ausgänge**

| Name | Typ | Beschreibung |
| --- | --- | --- |
| IN | STRING | Input data from Adapter |

### **Adapter**

| Rolle | Name | Typ | Beschreibung |
| --- | --- | --- | --- |
| Socket | AIS_IN | adapter::types::unidirectional::AIS | Adapter Input |

## Funktionsweise

Die Verbindungen stehen im Abschnitt „Verbindungen im Baustein“. Es gibt keine internen Funktionsbausteine, der Baustein verdrahtet nur Adapter und Ein-/Ausgänge.

## Verbindungen im Baustein

**Ereignisverbindungen:**

- `AIS_IN.E1` → `CNF`

**Datenverbindungen:**

- `AIS_IN.D1` → `IN`

## Technische Besonderheiten

- Initial Version - Luecke geschlossen (analog zu AB_B_TO_BYTE)

## ⚖️ Vergleich mit ähnlichen Bausteinen

Bausteine derselben Familie liegen im selben Ordner oder in den Nachbarordnern von `conversion`.

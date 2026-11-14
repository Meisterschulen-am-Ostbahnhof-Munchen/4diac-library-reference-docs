# AIS_STRING_TO_S

![AIS_STRING_TO_S](AIS_STRING_TO_S.svg)

![AIS_STRING_TO_S_network](./AIS_STRING_TO_S_network.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`AIS_STRING_TO_S` ist ein Composite-Baustein: Composite FB for convert STRING to AIS.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ | Mit | Beschreibung |
| --- | --- | --- | --- |
| REQ | Event | OUT | Request to Adapter |

### **Ereignis-Ausgänge**

Keine direkten Ereignisse.

### **Daten-Eingänge**

| Name | Typ | Beschreibung |
| --- | --- | --- |
| OUT | STRING | Output data to Adapter |

### **Daten-Ausgänge**

Keine direkten Daten.

### **Adapter**

| Rolle | Name | Typ | Beschreibung |
| --- | --- | --- | --- |
| Plug | AIS_OUT | adapter::types::unidirectional::AIS | Adapter Output |

## Funktionsweise

Die Verbindungen stehen im Abschnitt „Verbindungen im Baustein“. Es gibt keine internen Funktionsbausteine, der Baustein verdrahtet nur Adapter und Ein-/Ausgänge.

## Verbindungen im Baustein

**Ereignisverbindungen:**

- `REQ` → `AIS_OUT.E1`

**Datenverbindungen:**

- `OUT` → `AIS_OUT.D1`

## Technische Besonderheiten

- Initial Version - Luecke geschlossen (analog zu AB_BYTE_TO_B)

## ⚖️ Vergleich mit ähnlichen Bausteinen

Bausteine derselben Familie liegen im selben Ordner oder in den Nachbarordnern von `conversion`.

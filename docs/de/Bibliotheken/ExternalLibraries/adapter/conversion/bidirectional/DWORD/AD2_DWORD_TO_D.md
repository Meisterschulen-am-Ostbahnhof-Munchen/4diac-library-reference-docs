# AD2_DWORD_TO_D

![AD2_DWORD_TO_D](AD2_DWORD_TO_D.svg)

![AD2_DWORD_TO_D_network](./AD2_DWORD_TO_D_network.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`AD2_DWORD_TO_D` ist ein Composite-Baustein: Composite FB for convert DWORD to AD2.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ | Mit | Beschreibung |
| --- | --- | --- | --- |
| REQ | Event | OUT | Request to Adapter |

### **Ereignis-Ausgänge**

| Ereignis | Typ | Mit | Beschreibung |
| --- | --- | --- | --- |
| CNF | Event | IN | Confirmation from Adapter |

### **Daten-Eingänge**

| Name | Typ | Beschreibung |
| --- | --- | --- |
| OUT | DWORD | Output data to Adapter |

### **Daten-Ausgänge**

| Name | Typ | Beschreibung |
| --- | --- | --- |
| IN | DWORD | Input data from Adapter |

### **Adapter**

| Rolle | Name | Typ | Beschreibung |
| --- | --- | --- | --- |
| Plug | AD2_OUT | adapter::types::bidirectional::AD2 | Adapter Output |

## Funktionsweise

Die Verbindungen stehen im Abschnitt „Verbindungen im Baustein“. Es gibt keine internen Funktionsbausteine, der Baustein verdrahtet nur Adapter und Ein-/Ausgänge.

## Verbindungen im Baustein

**Ereignisverbindungen:**

- `REQ` → `AD2_OUT.EO1`
- `AD2_OUT.EI1` → `CNF`

**Datenverbindungen:**

- `OUT` → `AD2_OUT.DO1`
- `AD2_OUT.DI1` → `IN`

## Technische Besonderheiten

- Initial Version - Luecke geschlossen (analog zu AB2_BYTE_TO_B)

## ⚖️ Vergleich mit ähnlichen Bausteinen

Bausteine derselben Familie liegen im selben Ordner oder in den Nachbarordnern von `conversion`.

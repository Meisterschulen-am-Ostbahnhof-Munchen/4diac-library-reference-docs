# AD2_D_TO_DWORD

![AD2_D_TO_DWORD](AD2_D_TO_DWORD.svg)

![AD2_D_TO_DWORD_network](./AD2_D_TO_DWORD_network.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`AD2_D_TO_DWORD` ist ein Composite-Baustein: Composite FB for convert AD2 to DWORD.

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
| Socket | AD2_IN | adapter::types::bidirectional::AD2 | Adapter Input |

## Funktionsweise

Die Verbindungen stehen im Abschnitt „Verbindungen im Baustein“. Es gibt keine internen Funktionsbausteine, der Baustein verdrahtet nur Adapter und Ein-/Ausgänge.

## Verbindungen im Baustein

**Ereignisverbindungen:**

- `AD2_IN.EO1` → `CNF`
- `REQ` → `AD2_IN.EI1`

**Datenverbindungen:**

- `AD2_IN.DO1` → `IN`
- `OUT` → `AD2_IN.DI1`

## Technische Besonderheiten

- Initial Version - Luecke geschlossen (analog zu AB2_B_TO_BYTE)

## ⚖️ Vergleich mit ähnlichen Bausteinen

Bausteine derselben Familie liegen im selben Ordner oder in den Nachbarordnern von `conversion`.

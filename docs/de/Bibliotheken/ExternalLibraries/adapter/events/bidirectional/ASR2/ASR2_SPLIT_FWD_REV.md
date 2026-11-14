# ASR2_SPLIT_FWD_REV

![ASR2_SPLIT_FWD_REV](ASR2_SPLIT_FWD_REV.svg)

![ASR2_SPLIT_FWD_REV_network](./ASR2_SPLIT_FWD_REV_network.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`ASR2_SPLIT_FWD_REV` reicht den bidirektionalen Adapter `ASR2` von `IN` nach `OUT` durch (Passthrough) und spiegelt zusätzlich den **Vorwärtskanal** auf `FWD_OUT` und den **Rückwärtskanal** auf `REV_OUT`. Beide Spiegel sind unidirektionale `ASR`-Adapter.

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
| Socket | IN | adapter::types::bidirectional::ASR2 | bidirektionaler Eingang |
| Plug | OUT | adapter::types::bidirectional::ASR2 | bidirektionaler Ausgang (Passthrough) |
| Plug | FWD_OUT | adapter::types::unidirectional::ASR | spiegelt den Vorwaerts-Kanal (IN.EO*) |
| Plug | REV_OUT | adapter::types::unidirectional::ASR | spiegelt den Rueckwaerts-Kanal (OUT.EI*) |

## Funktionsweise

Alle Verbindungen sind direkte Ereignis- und Datenverbindungen, es gibt keine eigene Logik. Die Liste steht im Abschnitt „Verbindungen im Baustein“.

## Verbindungen im Baustein

**Ereignisverbindungen:**

- `IN.EO_SET` → `OUT.EO_SET`
- `IN.EO_SET` → `FWD_OUT.SET`
- `OUT.EI_SET` → `IN.EI_SET`
- `OUT.EI_SET` → `REV_OUT.SET`
- `IN.EO_RESET` → `OUT.EO_RESET`
- `IN.EO_RESET` → `FWD_OUT.RESET`
- `OUT.EI_RESET` → `IN.EI_RESET`
- `OUT.EI_RESET` → `REV_OUT.RESET`

## Technische Besonderheiten

- Reiner Verdrahtungsbaustein (Composite ohne interne Funktionsbausteine).
- Der Spiegel dient zum Abgreifen eines Kanals, ohne den Passthrough zu stören.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- Gleiche Familie: [AB2_SPLIT_FWD_REV](../AB2_SPLIT_FWD_REV.md) und weitere im übergeordneten Ordner.

# A2X2_SPLIT_FWD_REV

![A2X2_SPLIT_FWD_REV](A2X2_SPLIT_FWD_REV.svg)

![A2X2_SPLIT_FWD_REV_network](./A2X2_SPLIT_FWD_REV_network.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`A2X2_SPLIT_FWD_REV` reicht den bidirektionalen Adapter `A2X2` von `IN` nach `OUT` durch (Passthrough) und spiegelt zusätzlich den **Vorwärtskanal** auf `FWD_OUT` und den **Rückwärtskanal** auf `REV_OUT`. Beide Spiegel sind unidirektionale `A2X`-Adapter.

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
| Socket | IN | adapter::types::bidirectional::A2X2 | bidirektionaler Eingang |
| Plug | OUT | adapter::types::bidirectional::A2X2 | bidirektionaler Ausgang (Passthrough) |
| Plug | FWD_OUT | adapter::types::unidirectional::A2X | spiegelt den Vorwaerts-Kanal (IN.EO*) |
| Plug | REV_OUT | adapter::types::unidirectional::A2X | spiegelt den Rueckwaerts-Kanal (OUT.EI*) |

## Funktionsweise

Alle Verbindungen sind direkte Ereignis- und Datenverbindungen, es gibt keine eigene Logik. Die Liste steht im Abschnitt „Verbindungen im Baustein“.

## Verbindungen im Baustein

**Ereignisverbindungen:**

- `IN.EO_UP` → `OUT.EO_UP`
- `IN.EO_UP` → `FWD_OUT.E_UP`
- `OUT.EI_UP` → `IN.EI_UP`
- `OUT.EI_UP` → `REV_OUT.E_UP`
- `IN.EO_DOWN` → `OUT.EO_DOWN`
- `IN.EO_DOWN` → `FWD_OUT.E_DOWN`
- `OUT.EI_DOWN` → `IN.EI_DOWN`
- `OUT.EI_DOWN` → `REV_OUT.E_DOWN`

**Datenverbindungen:**

- `IN.DO_UP` → `OUT.DO_UP`
- `IN.DO_UP` → `FWD_OUT.UP`
- `OUT.DI_UP` → `IN.DI_UP`
- `OUT.DI_UP` → `REV_OUT.UP`
- `IN.DO_DOWN` → `OUT.DO_DOWN`
- `IN.DO_DOWN` → `FWD_OUT.DOWN`
- `OUT.DI_DOWN` → `IN.DI_DOWN`
- `OUT.DI_DOWN` → `REV_OUT.DOWN`

## Technische Besonderheiten

- Reiner Verdrahtungsbaustein (Composite ohne interne Funktionsbausteine).
- Der Spiegel dient zum Abgreifen eines Kanals, ohne den Passthrough zu stören.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- Gleiche Familie: [AB2_SPLIT_FWD_REV](../AB2_SPLIT_FWD_REV.md) und weitere im übergeordneten Ordner.

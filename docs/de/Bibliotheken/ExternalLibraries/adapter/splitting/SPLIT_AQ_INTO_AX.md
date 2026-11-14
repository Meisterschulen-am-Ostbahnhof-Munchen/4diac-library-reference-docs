# SPLIT_AQ_INTO_AX

![SPLIT_AQ_INTO_AX](SPLIT_AQ_INTO_AX.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`SPLIT_AQ_INTO_AX`: this Function splits the 2 AX (BOOL) adapters from a AQ (QUARTER).

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
| Socket | IN | adapter::types::unidirectional::AQ | QUARTER Input |
| Plug | BIT_00 | adapter::types::unidirectional::AX | Bool Bit 0 |
| Plug | BIT_01 | adapter::types::unidirectional::AX | Bool Bit 1 |

## Funktionsweise

Der Baustein ist ein Service-Interface-Baustein ohne eigenes Netzwerk im Typ. Seine Implementierung liegt in der Firmware.

## Technische Besonderheiten

- Initial Version - Luecke in der Adapter-Familie geschlossen
- AQ ist ein `QUARTER` mit 2 Bit, `AX` ein Bool-Adapter.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- Gegenstück: [ASSEMBLE_AQ_FROM_AX](../assembling/ASSEMBLE_AQ_FROM_AX.md).

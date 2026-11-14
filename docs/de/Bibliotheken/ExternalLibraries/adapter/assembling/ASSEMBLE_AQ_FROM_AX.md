# ASSEMBLE_AQ_FROM_AX

![ASSEMBLE_AQ_FROM_AX](ASSEMBLE_AQ_FROM_AX.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`ASSEMBLE_AQ_FROM_AX`: this Function combines the 2 AX (BOOL) adapters to a AQ (QUARTER).

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
| Socket | BIT_00 | adapter::types::unidirectional::AX | Bool Bit 0 |
| Socket | BIT_01 | adapter::types::unidirectional::AX | Bool Bit 1 |
| Plug | OUT | adapter::types::unidirectional::AQ | QUARTER Output (Bit 0 und 1) |

## Funktionsweise

Der Baustein ist ein Service-Interface-Baustein ohne eigenes Netzwerk im Typ. Seine Implementierung liegt in der Firmware.

## Technische Besonderheiten

- Initial Version - Luecke in der Adapter-Familie geschlossen
- AQ ist ein `QUARTER` mit 2 Bit, `AX` ein Bool-Adapter.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- Gegenstück: [SPLIT_AQ_INTO_AX](../splitting/SPLIT_AQ_INTO_AX.md).

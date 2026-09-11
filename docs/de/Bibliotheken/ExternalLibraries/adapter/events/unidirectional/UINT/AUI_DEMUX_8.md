# AUI_DEMUX_8

![AUI_DEMUX_8](./AUI_DEMUX_8.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AUI_DEMUX_8** ist ein Ereignis-Demultiplexer mit 8 Ausgängen. Er empfängt über einen AUI-Adapter (Latching-Adapter) sowohl das auslösende Ereignis als auch den Auswahlindex. Abhängig vom übergebenen Index wird das eingehende Ereignis an genau einen der acht Ereignis-Ausgänge weitergeleitet. Der Baustein ist als generischer FB (GenericClassName `GEN_E_DEMUX`) implementiert und stellt eine moderne, adapterbasierte Alternative zum klassischen `E_DEMUX` dar.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Baustein besitzt keine direkten Ereignis-Eingänge. Das Eingangsereignis wird über den AUI-Adapter `K` empfangen, der entsprechend der Adapterdefinition ein Ereignis sowie einen Datenwert (Index) bündelt.

### **Ereignis-Ausgänge**

| Name | Datentyp | Kommentar |
|------|----------|-----------|
| `EO1` | Event | Ausgang, demultiplext vom Eingangsereignis bei K = 0 |
| `EO2` | Event | Ausgang, demultiplext vom Eingangsereignis bei K = 1 |
| `EO3` | Event | Ausgang, demultiplext vom Eingangsereignis bei K = 2 |
| `EO4` | Event | Ausgang, demultiplext vom Eingangsereignis bei K = 3 |
| `EO5` | Event | Ausgang, demultiplext vom Eingangsereignis bei K = 4 |
| `EO6` | Event | Ausgang, demultiplext vom Eingangsereignis bei K = 5 |
| `EO7` | Event | Ausgang, demultiplext vom Eingangsereignis bei K = 6 |
| `EO8` | Event | Ausgang, demultiplext vom Eingangsereignis bei K = 7 |

### **Daten-Eingänge**

Es existieren keine direkten Daten-Eingänge. Der Index `K` wird als Datenwert über den AUI-Adapter bereitgestellt.

### **Daten-Ausgänge**

Der Baustein besitzt keine Daten-Ausgänge.

### **Adapter**

| Name | Typ | Richtung | Kommentar |
|------|-----|----------|-----------|
| `K` | `adapter::types::unidirectional::AUI` | Socket | Ereignisindex (Kanalauswahl) |

## Funktionsweise

Der Demultiplexer wartet auf ein Ereignis, das über den AUI-Adapter `K` eintrifft. Mit diesem Ereignis wird gleichzeitig ein Datenwert (Index) übertragen. Dieser Index bestimmt, welcher der acht Ausgänge `EO1` bis `EO8` aktiviert wird:

- Bei `K = 0` wird das Ereignis an `EO1` weitergegeben.
- Bei `K = 1` wird das Ereignis an `EO2` weitergegeben.
- …
- Bei `K = 7` wird das Ereignis an `EO8` weitergegeben.

Sobald das Ereignis eingetroffen und der Index ausgewertet ist, wird genau **ein** Ausgangsereignis erzeugt. Die Zuordnung ist deterministisch und ohne Verzögerung (kein interner Zustand).

## Technische Besonderheiten

- **Adapterbasierte Schnittstelle**  
  Der Baustein verwendet einen AUI-Adapter anstelle getrennter Eingänge für Ereignis (`EI`) und Index (`K`). Dies vereinfacht die Verbindung in Systemen, die bereits auf Adapterkommunikation aufgebaut sind, und erhöht die Wiederverwendbarkeit.

- **Generischer Baustein**  
  Über das Attribut `GenericClassName` wird auf den generischen FB `GEN_E_DEMUX` verwiesen. Dies ermöglicht eine flexible Parametrisierung und spätere Erweiterung (z. B. andere Demultiplexer-Breiten).

- **Keine Datenhaltung**  
  Der Baustein ist zustandslos. Es werden keine internen Variablen oder Zustände gespeichert, was ihn für hochdynamische Prozesse geeignet macht.

- **Vollständige Abdeckung des Indexbereichs**  
  Für die Werte 0 bis 7 sind alle Ausgänge definiert. Ein ungültiger Index (außerhalb dieses Bereichs) führt zu keinem Ausgangsereignis.

## Zustandsübersicht

Der Baustein besitzt keinen expliziten Zustandsautomaten. Das Verhalten ist rein kombinatorisch:

1. Warten auf Ereignis über den Adapter.
2. Empfang des Ereignisses inkl. Index.
3. Auswahl und Aktivierung des zugeordneten Ausgangs.
4. Rückkehr in den Wartezustand.

Es gibt keine internen Zustandsübergänge oder zeitlichen Bedingungen.

## Anwendungsszenarien

- **Verteilte Ereignisverarbeitung**  
  Ein zentrales Steuermodul sendet Ereignisse mit einem Prioritäts- oder Kanalindex. Der `AUI_DEMUX_8` leitet jedes Ereignis an die entsprechende Verarbeitungseinheit (z. B. verschiedene Regelungsblöcke) weiter.

- **Kanalumschaltung in Kommunikationssystemen**  
  In Nachrichten- oder Datenflussarchitekturen können über den Index unterschiedliche Empfänger adressiert werden, ohne dass die Verbindungslogik neu konfiguriert werden muss.

- **Steuerung von Mehrwegeventilen oder multiplexierten Aktoren**  
  Durch die Auswahl des Ausgangs können z. B. verschiedene Aktuatoren angesteuert werden, wobei nur ein Aktuator pro Ereignis reagieren soll.

## Vergleich mit ähnlichen Bausteinen

| Eigenschaft | `AUI_DEMUX_8` | `E_DEMUX` (klassisch) |
|-------------|---------------|------------------------|
| Ereigniseingang | Über Adapter `K` | Separater Eingang `EI` |
| Indexeingang | Über Adapter `K` (Datenkanal) | Separater Dateneingang `K` |
| Anzahl Ausgänge | 8 (EO1–EO8) | Konfigurierbar, z. B. 2, 4, 8 |
| Schnittstelle | Einheitlicher AUI-Adapter | Einzelne Ein-/Ausgänge |
| Generisch | Ja (über `GEN_E_DEMUX`) | Teilspezifisch |

Der wesentliche Unterschied liegt in der Zusammenführung von Ereignis und Index in einem Adapter. Dadurch wird die Verkabelung vereinfacht und die Schnittstelle für modulare Systeme standardisiert.

## Fazit

Der `AUI_DEMUX_8` ist ein leistungsfähiger, ereignisbasierter Demultiplexer, der die Vorteile moderner Adaptertechnik mit der Flexibilität generischer Bausteine verbindet. Durch die klare Zuordnung von Index zu Ausgang und die zustandslose Arbeitsweise eignet er sich ideal für viele industrielle Automatisierungs- und Kommunikationsszenarien. Seine adapterbasierte Schnittstelle reduziert Verdrahtungsaufwand und erhöht die Wartbarkeit der Gesamtapplikation.

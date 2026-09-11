# IG2_Device_Classes

![IG2_Device_Classes](./IG2_Device_Classes.svg)

* * * * * * * * * *
## Einleitung
Der GlobalConstants-Baustein **IG2_Device_Classes** definiert eine Sammlung von Konstanten für Geräteklassen (Device Classes) gemäß dem ISOBUS‑Standard (ISO 11783). Diese Konstanten sind für die Industriegruppe 2 (Industry Group 2) spezifisch und kennzeichnen die Art eines Fahrzeugsystems (z. B. Traktor, Erntemaschine, Anhänger). Sie werden typischerweise in der Kommunikation zwischen Traktor und Anbaugeräten verwendet, um die Typklassifizierung des angeschlossenen Geräts zu übermitteln.

## Schnittstellenstruktur
Da es sich um einen GlobalConstants-Baustein handelt, existieren keine Ein‑/Ausgänge, Ereignisse oder Adapter. Der Baustein stellt ausschließlich Konstanten bereit, die als globale Variablen in der IEC 61499-Umgebung genutzt werden können.

### **Ereignis-Eingänge**
Nicht vorhanden.

### **Ereignis-Ausgänge**
Nicht vorhanden.

### **Daten-Eingänge**
Nicht vorhanden.

### **Daten-Ausgänge**
Nicht vorhanden.

### **Adapter**
Nicht vorhanden.

## Funktionsweise
Der Baustein definiert eine Reihe von benannten Konstanten vom Typ **BYTE**, die jeweils einen bestimmten numerischen Wert und eine dazugehörige Bezeichnung besitzen. Die Werte entsprechen den offiziellen Geräteklassen‑Codierungen des ISOBUS-Standards. Sie werden typischerweise in Kommunikationsprotokollen verwendet, um die Art eines Fahrzeugs oder Anbaugeräts zu identifizieren. Beispielsweise steht der Wert `1` für **Tractor**, `7` für **Harvesters** und `127` für **Not Available**.

Die folgende Tabelle zeigt die definierten Konstanten:

| Konstante | Wert (BYTE) | Bedeutung |
|---|---|---|
| `DC_NON_SPECIFIC_SYSTEM` | 0 | Nicht spezifisches System |
| `DC_TRACTOR` | 1 | Traktor |
| `DC_TILLAGE` | 2 | Bodenbearbeitung |
| `DC_SECONDARY_TILLAGE` | 3 | Sekundäre Bodenbearbeitung |
| `DC_PLANTERS_SEEDERS` | 4 | Pflanzmaschinen / Sägeräte |
| `DC_FERTILIZERS` | 5 | Düngerstreuer |
| `DC_SPRAYERS` | 6 | Spritzgeräte |
| `DC_HARVESTERS` | 7 | Erntemaschinen |
| `DC_ROOT_HARVESTERS` | 8 | Wurzelfrüchte-Erntemaschinen |
| `DC_FORAGE` | 9 | Futtererntemaschinen |
| `DC_IRRIGATION` | 10 | Bewässerung |
| `DC_TRANSPORT_TRAILER` | 11 | Transport / Anhänger |
| `DC_FARM_YARD_OPERATIONS` | 12 | Hofwirtschaft |
| `DC_POWERED_AUXILIARY_DEVICES` | 13 | Angetriebene Hilfsgeräte |
| `DC_SPECIAL_CROPS` | 14 | Spezialkulturen |
| `DC_EARTH_WORK` | 15 | Erdarbeiten |
| `DC_SKIDDER` | 16 | Skidder (Rückeschlepper) |
| `DC_SENSOR_SYSTEMS` | 17 | Sensorsysteme |
| `DC_TIMBER_HARVESTERS` | 19 | Holzerntemaschinen |
| `DC_FORWARDERS` | 20 | Forwarder |
| `DC_TIMBER_LOADERS` | 21 | Holzverlader |
| `DC_TIMBER_PROCESSING_MACHINES` | 22 | Holzverarbeitungsmaschinen |
| `DC_MULCHERS` | 23 | Mulcher |
| `DC_UTILITY_VEHICLES` | 24 | Nutzfahrzeuge |
| `DC_SLURRY_MANURE_APPLICATORS` | 25 | Gülle‑/Mistausbringer |
| `DC_FEEDERS_MIXERS` | 26 | Fütterungs-/Mischgeräte |
| `DC_WEEDERS` | 27 | Unkrautbekämpfung (nicht chemisch) |
| `DC_TURF_AND_LAWN_CARE_MOWERS` | 28 | Rasen- und Rasenpflegemäher |
| `DC_PRODUCT_MATERIAL_HANDLING` | 29 | Produkt-/Materialhandhabung |
| `DC_NOT_AVAILABLE` | 127 | Nicht verfügbar |

## Technische Besonderheiten
- Die Konstanten sind als **BYTE** deklariert und besitzen feste Werte im Bereich von 0 bis 127.
- Der Wert `127` ist als `DC_NOT_AVAILABLE` reserviert und signalisiert, dass keine gültige Geräteklasse vorliegt.
- Einige Werte (z. B. 18, 30–126) sind im Standard nicht belegt und daher nicht definiert. Dies bietet Raum für zukünftige Erweiterungen.
- Der Baustein ist als GlobalConstants konzipiert und kann in FB‑Netzwerken oder SFC‑Programmen über den Namen direkt referenziert werden.
- Die Konstanten sind Teil der ISOBUS‑Datenkommunikation (PGN‑Konstanten) und erleichtern eine standardkonforme Implementierung.

## Zustandsübersicht
Für einen GlobalConstants-Baustein existiert keine Zustandsautomaten‑ oder Zustandslogik. Die Konstanten sind statische Werte und ändern sich während der Laufzeit nicht.

## Anwendungsszenarien
- **ISOBUS‑Kommunikation:** In einer ISOBUS‑Umgebung kann ein Steuergerät (ECU) die Geräteklasse seines Anbaugeräts mithilfe dieser Konstanten in den dafür vorgesehenen PGNs (Parameter Group Numbers) kodieren.
- **Typidentifikation:** Ein Traktor kann anhand dieser Konstanten automatisch erkennen, ob es sich bei einem angeschlossenen Gerät um einen Pflug, eine Sämaschine oder eine Erntemaschine handelt.
- **Diagnose und Konfiguration:** Bei der Inbetriebnahme oder Fehleranalyse kann die Geräteklasse zur schnellen Identifikation des Gerätetyps herangezogen werden.
- **Entwicklung von ISOBUS‑Anwendungen:** Bei der Erstellung von 4diac‑Applikationen können diese Konstanten direkt als symbolische Namen verwendet werden, anstatt magische Zahlenwerte zu verwenden.

## Vergleich mit ähnlichen Bausteinen
Es existieren weitere GlobalConstants‑Bausteine für andere Industriegruppen (z. B. `IG1_Device_Classes` für Industriegruppe 1), die ähnliche Konstanten für andere Anwendungsbereiche definieren. Der Unterschied liegt in den zugeordneten Werten und den spezifischen Geräteklassen. Während IG1 eher allgemeine industrielle Anwendungen abdeckt, fokussiert IG2 auf Fahrzeug‑ und Agrarsysteme.

## Fazit
Der GlobalConstants-Baustein **IG2_Device_Classes** stellt eine standardkonforme und wartungsfreundliche Möglichkeit dar, die Geräteklassen im ISOBUS‑Kontext zu verwenden. Durch die Bereitstellung symbolischer Namen wird die Lesbarkeit und Fehleranfälligkeit von Applikationen reduziert. Die fest definierten Werte gewährleisten Kompatibilität mit der ISOBUS‑Norm und ermöglichen eine klare Kommunikation zwischen verschiedenen Geräten.
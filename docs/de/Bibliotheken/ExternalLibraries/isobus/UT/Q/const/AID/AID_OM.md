# AID_OM

![AID_OM](./AID_OM.svg)

* * * * * * * * * *
## Einleitung

Die Globalkonstanten **AID_OM** definieren die Attribut-IDs (Attribute IDs) für das Objekt „Output Meter“ im Rahmen des ISOBUS‑Protokolls (ISO 11783). Sie dienen als einheitliche Referenzwerte für die Adressierung von Eigenschaften eines Anzeige‑Instruments (z. B. Rundinstrument) in der Benutzerschnittstelle eines Terminals. Die Werte sind als USINT (Unsigned Short Integer) im Bereich 1…12 festgelegt und werden typischerweise beim Zugriff auf Objektattribute über den ISOBUS‑Kommunikationsstack verwendet.

## Schnittstellenstruktur

**AID_OM** ist keine typische Funktionsblock‑ oder Adapter‑Instanz, sondern eine Sammlung globaler Konstanten. Daher sind keine ereignis‑ oder datenbasierten Ein‑/Ausgänge vorhanden.

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

Die Konstantenwerte repräsentieren die Attributkennungen (AIDs) des „Output Meter“‑Objekts. Sie werden in der Regel als **const**‑Werte in den Quellcode eingebunden, um die Lesbarkeit zu verbessern und die Gefahr von Tippfehlern bei der Verwendung numerischer Literale zu vermeiden. Jede Konstante steht für ein spezifisches Attribut:

| Konstantenname | Wert | Beschreibung |
|----------------|------|--------------|
| `WIDTH` | 1 | Breite des Instruments in Pixeln |
| `NEEDLE_COLOUR` | 2 | Farbindex für die Nadel |
| `BORDER_COLOUR` | 3 | Farbindex für den Rahmen |
| `ARC_THICK_COLOUR` | 4 | Farbe/Stärke des Kreisbogens |
| `OPTIONS` | 5 | Bitmaske für Optionen (z. B. Bogen, Rahmen, Skalenteilung, Drehrichtung) |
| `NUMB_TICKS` | 6 | Anzahl der Skalenteilstriche |
| `START_ANGLE` | 7 | Startwinkel des Skalenbogens |
| `END_ANGLE` | 8 | Endwinkel des Skalenbogens |
| `MIN_VALUE` | 9 | Minimaler darzustellender Wert |
| `MAX_VALUE` | 10 | Maximaler darzustellender Wert |
| `VARIABLE_REF` | 11 | Objekt‑ID einer Variablen, die den aktuellen Wert liefert |
| `VALUE` | 12 | Aktueller Wert des Instruments |

Die Konstanten werden als **globale Konstanten** in einem 4diac‑Projekt deklariert und können in FB‑Netzwerken oder ST‑Code referenziert werden.

## Technische Besonderheiten

- **Typ**: `USINT` (8‑Bit‑Unsigned Integer)
- **Initialwerte**: Die Werte sind direkt als USINT‑Literale vorgegeben (z. B. `USINT#1`).
- **Verwendung im ISOBUS**: Die AIDs sind Teil des ISO‑11783‑Teil 6 (Virtual Terminal) und werden zum Lesen/Schreiben von Objekteigenschaften verwendet.
- **Erweiterbarkeit**: Die Konstanten sind in einem eigenen Paket (`isobus::UT::Q::const::AID`) gekapselt, wodurch sie modular in verschiedene Module eingebunden werden können.
- **Lizenz**: Der Kommentar verweist auf die Eclipse Public License 2.0.

## Zustandsübersicht

Da es sich um reine Konstanten handelt, existieren keine Zustände. Die Werte sind während der gesamten Laufzeit unveränderlich (konstant).

## Anwendungsszenarien

- **Steuerung von Anzeigeinstrumenten** in landwirtschaftlichen Maschinen: Ein FB, der ein „Output Meter“ bedient, kann die Attribut‑IDs aus `AID_OM` verwenden, um z. B. den aktuellen Wert (`AID_OM.VALUE`) zu setzen oder die Skalierung (`AID_OM.MIN_VALUE`, `AID_OM.MAX_VALUE`) zu konfigurieren.
- **Entwicklung von ISOBUS‑Terminals**: Beim Implementieren der Virtual‑Terminal‑Schnittstelle (VT) erleichtern diese Konstanten die Zuordnung von Attributen zu den vom Standard geforderten Objekten.
- **Test‑ und Simulationsumgebungen**: Durch die zentrale Definition können Testfälle die korrekten AIDs unabhängig von konkreten Werten referenzieren.

## Vergleich mit ähnlichen Bausteinen

In der ISOBUS‑Welt existieren für jedes Objekt eigene Satelliten‑Konstantensammlungen, z. B. `AID_SO` (Softkey‑Objekt) oder `AID_WS` (Working Set). Diese folgen dem gleichen Muster: Jede Sammlung definiert numerische Konstanten für die Attribute des jeweiligen Objekttyps. `AID_OM` ist speziell für das Output‑Meter‑Objekt ausgelegt und unterscheidet sich durch die spezifischen Eigenschaften (Winkel, Ticks, Nadel etc.) von anderen Objekttypen.

## Fazit

Die Globalkonstanten `AID_OM` stellen eine saubere und typsichere Möglichkeit dar, die Attribut‑IDs für ISOBUS‑Output‑Meter‑Objekte zu definieren. Sie erhöhen die Wartbarkeit und Lesbarkeit des Codes, indem sie numerische Literale durch sprechende Namen ersetzen. Obwohl sie keinen direkten Funktionsblock darstellen, sind sie ein essenzieller Bestandteil für die Entwicklung von 4diac‑Anwendungen im Bereich der landwirtschaftlichen Automatisierung.
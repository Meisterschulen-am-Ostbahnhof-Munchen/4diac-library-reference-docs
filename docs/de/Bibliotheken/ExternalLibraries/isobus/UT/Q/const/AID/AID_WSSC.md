# AID_WSSC

![AID_WSSC](./AID_WSSC.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `AID_WSSC` ist eine Sammlung globaler Konstanten, die Attribut-Identifikationsnummern (Attribute IDs) für das Objekt "Working Set Special Controls" im ISOBUS-Kontext definiert. Diese Konstanten werden verwendet, um auf bestimmte Attribute eines Working Set Special Controls‑Objekts (WSSC) zu referenzieren, beispielsweise für Farbkarten oder Farbpaletten. Die Konstanten sind als `USINT`‑Werte (8‑Bit ohne Vorzeichen) definiert und in einem GlobalConstants‑Baustein gebündelt, sodass sie projektweit genutzt werden können.

## Schnittstellenstruktur

Da es sich um einen GlobalConstants-Baustein handelt, besitzt dieser keine Ereignis-, Daten- oder Adapter-Schnittstellen im klassischen Sinne eines Funktionsblocks. Die bereitgestellten Konstanten dienen als statische Werte und werden nicht über Ein-/Ausgänge ausgetauscht. Nachfolgend sind die definierten Konstanten aufgelistet:

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

**Bereitgestellte Konstanten:**

| Konstante         | Typ   | Wert | Kommentar                                                       |
|-------------------|-------|------|-----------------------------------------------------------------|
| `NUMOFBYTES`      | USINT | 1    | Anzahl der folgenden Bytes im Objekt (AID_WSSC_NUMOFBYTES).     |
| `COLOUR_MAP`      | USINT | 2    | Objekt‑ID einer Farbkarte (Colour Map) oder `NULL` (0).         |
| `COLOUR_PALETTE`  | USINT | 3    | Objekt‑ID einer Farbpalette (Colour Palette) oder `NULL` (0).   |

## Funktionsweise

Der Baustein stellt die Konstantenwerte für die Attribut-IDs bereit, die im ISOBUS‑Protokoll (ISO 11783) für das Objekt "Working Set Special Controls" verwendet werden. Diese IDs werden typischerweise bei der Erstellung oder Übertragung von Nachrichten referenziert, um auf die entsprechenden Attribute eines Geräts zuzugreifen. Durch die Definition als globale Konstanten wird deren konsistente Verwendung im gesamten Projekt sichergestellt und Fehler durch hartkodierte Werte vermieden.

## Technische Besonderheiten

- **ISOBUS‑Standard**: Die Konstanten entsprechen den Attribut-IDs, die im ISO 11783‑Teil 6 (Virtual Terminal) für WSSC-Objekte spezifiziert sind.
- **Datentyp**: Alle Werte sind vom Typ `USINT` (Unsigned Short Integer) mit einem Wertebereich von 0–255.
- **Initialwerte**: Die Konstanten sind mit festen Werten initialisiert und können nicht zur Laufzeit verändert werden.
- **Lizenz**: Der Baustein ist unter der Eclipse Public License 2.0 (EPL-2.0) veröffentlicht, was die Nutzung in Open‑Source‑ und proprietären Projekten ermöglicht.
- **Paketzuordnung**: Der Baustein befindet sich im Paket `isobus::UT::Q::const::AID`, was eine klare strukturelle Einordnung in einem ISOBUS‑Projekt ermöglicht.

## Zustandsübersicht

Der Baustein besitzt keinerlei Zustände, da es sich um eine rein deklarative Ansammlung von Konstanten handelt. Es existieren keine internen Variablen oder Ablaufsteuerungen, die einen Zustandsübergang erfordern würden.

## Anwendungsszenarien

- **ISOBUS‑Implementierung**: Verwendung in der Kommunikation zwischen Traktor und Anbaugerät, insbesondere bei der Übertragung von Arbeitsbildschirmen oder Maschinensteuerungen, die Working Set Special Controls verwenden.
- **Entwicklung von Virtual‑Terminal‑Applikationen**: Referenzierung der konstanten Werte beim Aufbau von Nachrichten, die Attribute von WSSC-Objekten ansprechen.
- **Konfiguration von Farbdarstellungen**: Nutzung von `COLOUR_MAP` und `COLOUR_PALETTE`, um Farbkarten oder Farbpaletten einem WSSC zuordnen zu können.

## Vergleich mit ähnlichen Bausteinen

Im ISOBUS‑Kontext gibt es weitere GlobalConstants‑Bausteine, die Attribut-IDs für andere Objekttypen definieren, z. B. für "Object Pool" oder "Working Set". Im Vergleich zu diesen Bausteinen fokussiert `AID_WSSC` ausschließlich auf die Attribute, die für Working Set Special Controls relevant sind. Die Struktur ist identisch: Eine Auflistung von `USINT`‑Konstanten mit Initialwerten, die die jeweiligen Attributed-IDs repräsentieren. Der Unterschied liegt lediglich in der semantischen Bedeutung der Werte für das jeweilige Objekt.

## Fazit

`AID_WSSC` ist ein zweckmäßiger GlobalConstants-Baustein, der die Attribut-IDs für Working Set Special Controls im ISOBUS bereitstellt. Er trägt zur Codeklarheit und Wartbarkeit bei, indem er die Verwendung fester Zahlenwerte an zentraler Stelle bündelt. Durch seine einfache Struktur – nur drei Konstanten – ist er leicht zu integrieren und in Projekten einzusetzen, die eine ISOBUS‑Kommunikation auf Basis des Virtual‑Terminal‑Protokolls implementieren.

# AID_PG

![AID_PG](./AID_PG.svg)

* * * * * * * * * *
## Einleitung

Der Baustein `AID_PG` ist ein **GlobalConstants**-Element der 4diac-IDE, das die Attribut-IDs (Attribute IDs) für ein **Picture Graphic Objekt** im ISOBUS-Kontext definiert. Diese Konstanten werden verwendet, um auf spezifische Attribute eines Bildgrafikelements in der Steuerungskommunikation zu referenzieren (z. B. Breite, Höhe, Format). Der Baustein stellt keine typischen Ein-/Ausgänge eines Funktionsblocks bereit, sondern dient als zentrale Konstante, die global in der Applikation verfügbar ist.

## Schnittstellenstruktur

Da es sich um einen **GlobalConstants**-Baustein handelt, besitzt er **keine** klassischen Ein-/Ausgangsschnittstellen. Die definierten Werte sind als globale Konstanten fest verdrahtet und können an beliebigen Stellen im Projekt verwendet werden.

### **Ereignis-Eingänge**

Keine vorhanden.

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

Keine vorhanden.

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

Keine vorhanden.

## Funktionsweise

Der Baustein definiert sechs globale Konstanten, die jeweils eine bestimmte Attribut-ID für ein Picture Graphic Objekt repräsentieren. Diese Konstanten werden im Compiler-Paket `isobus::UT::Q::const::AID` gebündelt und können an jedem Punkt eines 4diac-Projekts referenziert werden. Die Zuordnung der ID-Werte ist fest in der ISOBUS-Spezifikation verankert.

| Konstante           | Wert (USINT) | Bedeutung                                                                 |
|---------------------|--------------|---------------------------------------------------------------------------|
| `WIDTH`             | `USINT#1`    | AID_PG_WIDTH – Breite des Bildes in Pixeln.                               |
| `OPTIONS`           | `USINT#2`    | AID_PG_OPTIONS – Bitmaske: Bit 0 = transparent, Bit 1 = blinkend, Bit 2 = RLE-codiert. |
| `TRANSPARENCY_COLOUR`| `USINT#3`    | AID_PG_TRANSPARENCY_COLOUR – Transparenzfarbe (falls transparent).       |
| `ACTUAL_WIDTH`      | `USINT#4`    | AID_PG_ACTUAL_WIDTH – Tatsächliche Breite des Bildes.                     |
| `ACTUAL_HEIGHT`     | `USINT#5`    | AID_PG_ACTUAL_HEIGHT – Tatsächliche Höhe des Bildes.                      |
| `FORMAT`            | `USINT#6`    | AID_PG_FORMAT – Bildtyp: 0 = Monochrom, 1 = 4-Bit-Farbe, 2 = 8-Bit-Farbe. |

Diese Konstanten werden typischerweise verwendet, um beim Zugriff auf ein Picture Graphic Objekt über den ISOBUS-Datenaustausch die entsprechenden Attribut-IDs eindeutig zu identifizieren.

## Technische Besonderheiten

- Der Baustein ist als **GlobalConstants**-Element implementiert, nicht als Funktionsblock. Er definiert keine ausführbare Logik, sondern nur eine feste Sammlung von Konstanten.
- Die Werte sind als `USINT` (Unsigned Short Integer) deklariert und mit konkreten Initialwerten versehen.
- Die Konstanten sind im Paket `isobus::UT::Q::const::AID` gekapselt, was eine klare Namensraum-Trennung innerhalb der ISOBUS-Bibliothek ermöglicht.
- Das Element enthält Metadaten wie Versionsinformationen und einen Original-Quellcode-Abschnitt (`OriginalSource`), der die Definition in der 4diac-IDE dokumentiert.

## Zustandsübersicht

Nicht anwendbar, da es sich um einen Konstanten-Baustein handelt und keine Zustände oder Zustandsübergänge existieren.

## Anwendungsszenarien

Typische Einsatzgebiete sind:

- **ISOBUS-Terminals und -Steuerungen**, die Bildgrafiken (Picture Graphics) für die Darstellung von Symbolen oder Icon-Informationen benötigen.
- **Datenzugriff** auf Attribute eines Bildobjekts, z. B. um die Breite oder das Format eines Bildes zu ermitteln oder zu setzen.
- **Erweiterung von HMI-Anwendungen** im landwirtschaftlichen Maschinenbereich, die ISOBUS-konforme Darstellungen verwenden (z. B. gemäß ISO 11783).

## Vergleich mit ähnlichen Bausteinen

In der ISOBUS-Bibliothek existieren weitere `GlobalConstants`-Elemente, die ähnliche Attribut-IDs für andere Objekttypen definieren (z. B. `AID_OBJ` für generische Objekte, `AID_BUTTON` für Tasten, `AID_LABEL` für Beschriftungen). Diese unterscheiden sich hauptsächlich durch die spezifischen Attribut-IDs und deren Bedeutung. Der vorliegende Baustein `AID_PG` ist speziell auf **Picture Graphs** ausgelegt und stellt nur die für diesen Objekttyp relevanten Konstanten bereit.

## Fazit

`AID_PG` ist ein einfacher, aber wichtiger Konstanten-Baustein für die ISOBUS-Kommunikation. Er bündelt die definierten Attribut-IDs für Bildgrafiken in einer zentralen, wiederverwendbaren Form und erleichtert so die Entwicklung von 4diac-Applikationen, die mit ISOBUS-Bildobjekten interagieren. Trotz des Fehlens von Schnittstellen ist er ein unverzichtbarer Bestandteil für die korrekte Parametrierung von Bildgrafiken im landwirtschaftlichen Automatisierungsumfeld.
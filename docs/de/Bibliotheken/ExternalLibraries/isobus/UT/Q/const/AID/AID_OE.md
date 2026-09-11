# AID_OE

![AID_OE](./AID_OE.svg)

* * * * * * * * * *

## Einleitung

Der Baustein AID_OE ist ein globaler Konstanten-Container (GlobalConstants) aus der 4diac-IDE, der die Objektattribut-IDs (Attribute IDs) für ein „Output Ellipse“-Objekt im ISOBUS-Standard (ISO 11783) definiert. Diese Konstanten werden verwendet, um Attribute wie Linienstärke, Größe, Ellipsentyp oder Füllbereich eindeutig zu identifizieren. Der Baustein stellt keine eigene Funktionalität bereit, sondern dient als zentrale Definitionsquelle für andere Bausteine im System.

## Schnittstellenstruktur

Da AID_OE ausschließlich globale Konstanten bereitstellt, besitzt er keine ereignis- oder datenbasierten Ein-/Ausgänge im klassischen Sinne. Die folgenden Abschnitte sind daher leer bzw. nicht zutreffend.

### **Ereignis-Eingänge**

Keine vorhanden.

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

Keine vorhanden.

### **Daten-Ausgänge**

Der Baustein stellt die folgenden Konstanten als feste Werte bereit, die als „Daten-Ausgänge“ interpretiert werden können:

| Konstante | Typ | Wert | Beschreibung |
|-----------|-----|------|--------------|
| `LINE_ATT` | USINT | 1 | Objekt-ID eines Linien-Attribut-Objekts (Line Attributes) |
| `WIDTH`    | USINT | 2 | Breite in Pixeln |
| `HEIGHT`   | USINT | 3 | Höhe in Pixeln |
| `ELLIPSE_TYPE` | USINT | 4 | Ellipsentyp: 0 = geschlossene Ellipse, 1 = offene Ellipse, 2 = geschlossenes Segment, 3 = geschlossener Sektor |
| `START_ANGLE` | USINT | 5 | Startwinkel der Ellipse |
| `END_ANGLE`   | USINT | 6 | Endwinkel der Ellipse |
| `FILL_ATT`    | USINT | 7 | Objekt-ID eines Füll-Attribut-Objekts (Fill Attributes) |

### **Adapter**

Keine vorhanden.

## Funktionsweise

AID_OE definiert sieben numerische Konstanten vom Typ `USINT` (Unsigned Short Integer), die jeweils eine spezifische Attribut-ID für ein Ellipsen-Objekt im ISOBUS-Kontext repräsentieren. Diese IDs werden in anderen Funktionsbausteinen verwendet, um auf die entsprechenden Eigenschaften eines grafischen Ellipsenelements zuzugreifen oder diese zu setzen. Da es sich um globale Konstanten handelt, sind die Werte während der gesamten Laufzeit unveränderlich und stehen allen beteiligten Bausteinen zur Verfügung.

## Technische Besonderheiten

- Die Konstanten sind als `USINT` (8-Bit ohne Vorzeichen) definiert und besitzen feste Initialwerte.
- Der gesamte Definitionsblock ist unter dem Paketnamen `isobus::UT::Q::const::AID` abgelegt, was eine klare Namensraumzuordnung für ISOBUS-Anwendungen ermöglicht.
- Die Konstanten sind im IEC 61499-Standard als `VAR_GLOBAL CONSTANT` deklariert, wodurch sie global und unveränderlich sind.
- Der Baustein ist als `GlobalConstants`-Block modelliert und besitzt daher keine Zustandslogik oder dynamische Schnittstellen.

## Zustandsübersicht

Da es sich um einen reinen Konstantencontainer handelt, existiert kein Zustandsautomat. Der Baustein hat keinen internen Zustand und führt keine Zustandsübergänge aus.

## Anwendungsszenarien

- **Grafische Objektverwaltung**: In ISOBUS-Terminals werden Ellipsen (z.B. zur Darstellung von Bodenbearbeitungsspuren) gezeichnet. Die Attribut-IDs aus AID_OE werden verwendet, um die Geometrie (Breite, Höhe) und Darstellung (Linienart, Füllung) dieser Objekte zu parameterisieren.
- **Konfiguration von Arbeitsgeräten**: In landwirtschaftlichen Maschinen können über diese IDs die Attribute eines Ellipsen-Objekts gesetzt oder ausgelesen werden, z.B. für die Anzeige von Feldgrenzen oder Arbeitsbreiten.
- **Entwicklung von ISOBUS-Kommunikationsmodulen**: Zusammen mit anderen Konstantenblöcken (z.B. für Linien, Punkte) bildet AID_OE die Basis für die Implementierung von Objekt-Pools (Object Pool) gemäß ISO 11783-6.

## Vergleich mit ähnlichen Bausteinen

Ähnliche Konstantenblöcke existieren für andere grafische Grundobjekte, beispielsweise:

- **AID_OL** (Line): Objektattribut-IDs für Linienobjekte.
- **AID_OT** (Text): Objektattribut-IDs für Textobjekte.
- **AID_OR** (Rectangle): Objektattribut-IDs für Rechtecke.

Diese Blöcke folgen demselben Muster: Sie definieren eine Sammlung von USINT-Konstanten, die die Attribute des jeweiligen Objekttyps identifizieren. Sie unterscheiden sich lediglich in der spezifischen Bedeutung und Anzahl der Konstanten.

## Fazit

Der Baustein AID_OE stellt eine wichtige, klar strukturierte Konstantensammlung für die Attribut-IDs von Ellipsen-Objekten im ISOBUS-Bereich bereit. Durch seine globale Verfügbarkeit und die festen Werte unterstützt er eine konsistente und fehlerfreie Entwicklung von Anwendungen, die grafische Objekte nach ISO 11783 handhaben müssen. Auch wenn er selbst keine aktive Logik ausführt, ist er ein unverzichtbarer Bestandteil für die modulare und wartbare Implementierung entsprechender Steuerungssysteme.

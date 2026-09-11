# AID_OP_POLY

![AID_OP_POLY](./AID_OP_POLY.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `AID_OP_POLY` ist eine globale Konstantendefinition, die die Attribut-IDs für ein Ausgabe-Polygonobjekt im ISOBUS-Datenmodell festlegt. Er wird verwendet, um auf standardisierte Attribute wie Breite, Höhe, Linienattribute, Füllattribute und Polygontyp zuzugreifen. Diese Konstanten erleichtern die eindeutige Identifizierung und den Zugriff auf die entsprechenden Attribute in Kommunikationsprotokollen und Anwendungen der Agrartechnik.

## Schnittstellenstruktur

Da es sich um eine reine Konstantendefinition handelt, besitzt `AID_OP_POLY` keine Ein- oder Ausgangsvariablen im Sinne eines Funktionsblocks. Die Struktur besteht ausschließlich aus globalen Konstanten.

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

Der Baustein definiert fünf globale Konstanten vom Typ `USINT`, die jeweils eine eindeutige Attribut-Identifikationsnummer repräsentieren:

| Konstante        | Wert | Beschreibung                                                                 |
|------------------|------|------------------------------------------------------------------------------|
| `WIDTH`          | `1`  | Attribut `AID_OP_POLY_WIDTH` – Breite des Polygons in Pixeln.                |
| `HEIGHT`         | `2`  | Attribut `AID_OP_POLY_HEIGHT` – Höhe des Polygons in Pixeln.                 |
| `LINE_ATT`       | `3`  | Attribut `AID_OP_POLY_LINE_ATT` – Objekt-ID eines Linienattribut-Objekts.    |
| `FILL_ATT`       | `4`  | Attribut `AID_OP_POLY_FILL_ATT` – Objekt-ID eines Füllattribut-Objekts.      |
| `POLYGON_TYPE`   | `5`  | Attribut `AID_OP_POLY_POLYGON_TYPE` – Polygontyp (0=konvex, 1=nicht-konvex, 2=komplex, 3=offen). |

Die Konstanten können an beliebiger Stelle im Programm referenziert werden, um auf die zugehörigen Attribute zuzugreifen. Sie dienen als symbolische Namen für die numerischen Positionswerte im Attributrahmen des entsprechenden ISOBUS-Objekts.

## Technische Besonderheiten

- Die Konstanten sind als `GLOBALCONSTANTS` deklariert und damit innerhalb des gesamten Projekts gültig.
- Die Werte sind fest vorgegeben und können nicht zur Laufzeit verändert werden (Konstanten).
- Der Baustein ist gemäß IEC 61499-1 strukturiert und enthält Metadaten wie Version und Beschreibung (in der XML-Datei). Die Lizenzierung erfolgt unter der Eclipse Public License 2.0.
- Die Konstantenwerte entsprechen den standardisierten Attribut-IDs für Ausgabe-Polygone im ISOBUS (ISO 11783) Protokoll.

## Zustandsübersicht

Nicht zutreffend – der Baustein besitzt keine Zustände oder Zustandsautomaten.

## Anwendungsszenarien

- **ISOBUS-Terminals:** Verwendung bei der Darstellung von geometrischen Formen (z. B. Feldgrenzen, Fahrspuren) auf einem landwirtschaftlichen Terminal.
- **Steuerungsanwendungen:** Zugriff auf die Attribute eines Ausgabe-Polygons, um dessen Darstellung (Breite, Höhe, Linien, Füllung) dynamisch zu konfigurieren.
- **Protokollimplementierungen:** Erstellung und Interpretation von ISOBUS-Nachrichten, die Polygonattribute enthalten.

Durch die Verwendung dieser Konstanten wird der Code lesbarer und unabhängig von magischen Zahlen.

## Vergleich mit ähnlichen Bausteinen

Es gibt ähnliche globale Konstantendefinitionen für andere Objekttypen, z. B. `AID_OP_LINE` oder `AID_OP_RECTANGLE`, die jeweils die Attribut-IDs für Linien- oder Rechteckobjekte bereitstellen. Der Unterschied liegt in den spezifischen Attributen: Während ein Rechteck z. B. nur Breite und Höhe hat, besitzt ein Polygon zusätzlich Linien- und Füllattribute sowie einen Polygontyp. `AID_OP_POLY` stellt die für Polygone relevanten IDs bereit.

## Fazit

`AID_OP_POLY` ist eine einfache, aber essentielle Sammlung von Konstanten für die Arbeit mit ISOBUS-Ausgabe-Polygonen. Sie ermöglicht eine klare und standardisierte Referenzierung der Attribute und trägt zur Wartbarkeit und Portabilität von Anwendungen in der Agrartechnik bei. Die Verwendung symbolischer Namen reduziert Fehlerquellen und erleichtert die Einhaltung des ISOBUS-Standards.

# AID_IL

![AID_IL](./AID_IL.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `AID_IL` ist ein Global-Constants-Baustein, der die Attribut-IDs für ein **Input-List-Objekt** im ISOBUS-Kontext (ISO 11783) definiert. Diese Konstanten werden benötigt, um in der Kommunikation mit einem Universal Terminal (UT) gezielt auf Eigenschaften einer Input-Liste zuzugreifen, z. B. um deren Größe, aktuelle Auswahl oder Optionen zu setzen oder zu lesen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Nicht vorhanden.

### **Ereignis-Ausgänge**

Nicht vorhanden.

### **Daten-Eingänge**

Es sind keine beschreibbaren Daten-Eingänge vorhanden. Der Baustein stellt jedoch die folgenden globalen Konstanten bereit, die in der gesamten Applikation gelesen werden können:

| Konstante | Typ | Wert | Beschreibung |
|-----------|-----|------|--------------|
| `WIDTH` | `USINT` | `1` | AID_IL_WIDTH – Breite des Objekts in Pixeln. |
| `HEIGHT` | `USINT` | `2` | AID_IL_HEIGHT – Höhe des Objekts in Pixeln. |
| `VARIABLE_REF` | `USINT` | `3` | AID_IL_VARIABLE_REF – Objekt-ID einer Number-Variable, die der Input-Liste zugeordnet ist. |
| `VALUE` | `USINT` | `4` | AID_IL_VALUE – Ausgewählter Listenindex (0 bis 254); `255` bedeutet, dass kein Element ausgewählt ist. |
| `OPTIONS` | `USINT` | `5` | AID_IL_OPTIONS – Bitmaske: Bit 0 = aktiviert (Enabled), Bit 1 = Echtzeit-Bearbeitung (Real-time editing). |

### **Daten-Ausgänge**

Nicht vorhanden.

### **Adapter**

Nicht vorhanden.

## Funktionsweise

`AID_IL` stellt fünf symbolische Konstanten bereit, die als Attribut-IDs für ein Input-List-Objekt im ISOBUS-Universal-Terminal dienen. Die IDs sind als `USINT`-Werte fest definiert und unveränderlich. Sie werden typischerweise zusammen mit ISOBUS-Funktionsbausteinen verwendet, um Attributoperationen (z. B. Setzen oder Abfragen von Attributen) auf ein Input-List-Objekt zu adressieren.

Die Konstanten decken die wichtigsten Attribute einer Input-Liste ab:

- **Geometrie** (`WIDTH`, `HEIGHT`)
- **Datenanbindung** (`VARIABLE_REF`)
- **Zustand** (`VALUE`)
- **Verhalten** (`OPTIONS`)

Durch die Verwendung dieser Konstanten wird die Lesbarkeit und Wartbarkeit des Applikationscodes verbessert, da anstelle von magischen Zahlen aussagekräftige Namen verwendet werden können.

## Technische Besonderheiten

- Die Konstanten sind im Paket `isobus::UT::Q::const::AID` abgelegt.
- Sie sind als **globale Konstanten** definiert und können applikationsweit ohne zusätzliche Instanziierung verwendet werden.
- Alle Werte sind vom Typ `USINT` (8-Bit unsigned integer).
- Der Baustein enthält weder Zustandslogik noch prozedurale Anteile; er ist rein deklarativer Natur.

## Zustandsübersicht

Nicht zutreffend, da es sich um einen reinen Konstanten-Baustein ohne Laufzeitlogik handelt.

## Anwendungsszenarien

- **ISOBUS-Applikationen mit Input-Listen:** Zugriff auf Eigenschaften einer Input-Liste über die definierten Attribut-IDs.
- **Parametrierung von UT-Objekten:** Setzen der Breite, Höhe oder Optionen einer Input-Liste zur Laufzeit.
- **Auswertung von Benutzereingaben:** Lesen des aktuell ausgewählten Listeneintrags über die Konstante `VALUE`.
- **Kommunikation mit Universal Terminal:** Verwendung der Konstanten in Kombination mit ISOBUS-Funktionsbausteinen, die Attributoperationen auf Objekte ausführen.

## Vergleich mit ähnlichen Bausteinen

Im ISOBUS-Standard besitzen verschiedene Objekttypen eigene Attribut-IDs. `AID_IL` ist speziell für **Input-List-Objekte** ausgelegt. Vergleichbare Global-Constants-Bausteine existieren für andere Objekttypen, z. B. für Number-Variablen, Output-Listen oder grafische Objekte. Der grundsätzliche Aufbau und die Verwendung sind ähnlich; die Werte der Attribut-IDs unterscheiden sich jedoch je nach Objekttyp.

## Fazit

`AID_IL` ist eine kompakte und klar strukturierte Konstanten-Sammlung für die Arbeit mit ISOBUS-Input-Listen. Sie ermöglicht eine saubere, lesbare und standardkonforme Programmierung, indem sie die relevanten Attribut-IDs benennt und dokumentiert. Der Baustein ist besonders für Entwickler von ISOBUS-Applikationen nützlich, die mit Universal-Terminals und Input-Listen arbeiten.

# AID_OLBG

![AID_OLBG](./AID_OLBG.svg)

* * * * * * * * * *

## Einleitung

Der Baustein **AID_OLBG** (Attribute IDs for Output Linear Bar Graph) ist eine globale Konstantendefinition für den ISOBUS (ISO 11783) Standard. Er dient der Bereitstellung numerischer Kennungen (Attribute IDs), die benötigt werden, um auf die Attribute eines Objekts vom Typ *Output Linear Bar Graph* (lineares Balkendiagramm) zuzugreifen. Diese Konstanten werden typischerweise in Funktionsbausteinen verwendet, die ISOBUS-Objekte über das Netzwerk konfigurieren oder auslesen.

## Schnittstellenstruktur

Da es sich um eine reine Konstantendefinition handelt, besitzt dieser Baustein keine Ereignis- oder Datenein-/ausgänge sowie keine Adapter. Stattdessen werden die folgenden globalen Konstanten definiert:

| Konstante | Wert (USINT) | Bedeutung |
|-----------|--------------|-----------|
| `WIDTH`   | 1            | Breite des Balkendiagramms in Pixeln. |
| `HEIGHT`  | 2            | Höhe des Balkendiagramms in Pixeln. |
| `COLOUR`  | 3            | Farbindex für das Diagramm. |
| `TARGET_LINE_COLOUR` | 4 | Farbe der Ziellinie (Target Line). |
| `OPTIONS` | 5            | Bitmaske, die verschiedene Optionen festlegt (z. B. Rand zeichnen, Ziellinie, Ticks, Typ, Ausrichtung, Richtung). |
| `NUMB_TICKS` | 6        | Anzahl der anzuzeigenden Ticks (Skalenteilungen). |
| `MIN_VALUE` | 7          | Minimalwert des darzustellenden Bereichs. |
| `MAX_VALUE` | 8          | Maximalwert des darzustellenden Bereichs. |
| `VARIABLE_REF` | 9       | Objekt-ID einer Variablenreferenz, die den aktuellen Wert liefert. |
| `TARGET_VAL_VAR_REF` | 10 | Objekt-ID einer Variablenreferenz für den Zielwert. |
| `TARGET_VALUE` | 11     | Der gewünschte Zielwert (statisch). |
| `VALUE`      | 12         | Der aktuelle Wert des Balkendiagramms. |

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

Die hier definierten Konstanten sind numerische Kennungen (Attribute IDs), die im ISOBUS-Protokoll verwendet werden, um auf die Eigenschaften eines linearen Balkendiagramm-Objekts zuzugreifen. Jede Konstante entspricht einem bestimmten Attribut, das entweder gesetzt oder ausgelesen werden kann. Durch die Verwendung benannter Konstanten wird der Code lesbarer und weniger fehleranfällig, da magische Zahlen vermieden werden.

Die Konstanten sind als `USINT` (Unsigned Short Integer) mit den Werten 1 bis 12 deklariert und liegen im Namensraum `isobus::UT::Q::const::AID`. Sie sind Teil einer Gruppe von Global Constants, die in 4diac-IDE Projekten für die Entwicklung von ISOBUS-Anwendungen verwendet werden.

## Technische Besonderheiten

- Alle Konstanten sind als `USINT` (8-Bit unsigned) definiert und werden mit einem initialen Wert deklariert.
- Sie sind als `GLOBAL CONSTANT` gekennzeichnet, d. h. sie sind während der gesamten Laufzeit unveränderlich.
- Die Konstanten sind in einem speziellen Paket (`isobus::UT::Q::const::AID`) organisiert, um eine klare Zuordnung zum ISOBUS-Standard zu ermöglichen.
- Die Werte 1–12 entsprechen den Attribut-IDs des Objekts *Output Linear Bar Graph* nach ISO 11783-6.

## Zustandsübersicht

Dieser Baustein besitzt keine Zustandsmaschine und keinen internen Zustand. Es handelt sich um eine reine Deklarationsdatei, die zur Compilezeit ausgewertet wird.

## Anwendungsszenarien

- **ISOBUS-Terminal-Bedienoberflächen**: Konfiguration eines linearen Balkendiagramms zur Anzeige von Maschinenparametern (z. B. Füllständen, Temperaturen).
- **Eingebettete Steuerungen**: Kommunikation mit ISOBUS-fähigen Steuergeräten, um Attribute wie Breite, Farbe, Min/Max-Werte zu setzen.
- **Entwicklung mit 4diac IDE**: Einbinden der Konstanten in IEC 61499-Funktionsbausteine, die ISOBUS-Objekte manipulieren.

## Vergleich mit ähnlichen Bausteinen

Es gibt ähnliche globale Konstantendefinitionen für andere ISOBUS-Objekttypen, z. B. `AID_OB` (Output Bar Graph), `AID_OT` (Output Text) usw. Diese unterscheiden sich in den Namen und der Anzahl der Attribute. `AID_OLBG` ist speziell auf lineare Balkendiagramme zugeschnitten und enthält Attribute wie `TARGET_LINE_COLOUR` und `TARGET_VALUE`, die bei anderen Balkendiagrammtypen möglicherweise nicht existieren.

## Fazit

Die globale Konstantendefinition `AID_OLBG` stellt eine saubere und standardisierte Möglichkeit dar, die Attribut-IDs eines linearen Balkendiagramms im ISOBUS-Umfeld zu referenzieren. Sie erhöht die Wartbarkeit und Lesbarkeit von 4diac-Projekten und unterstützt Entwickler bei der Implementierung von ISOBUS-Anwendungen. Durch die klar definierten Konstanten können Fehler durch falsche Zahlenwerte vermieden werden.

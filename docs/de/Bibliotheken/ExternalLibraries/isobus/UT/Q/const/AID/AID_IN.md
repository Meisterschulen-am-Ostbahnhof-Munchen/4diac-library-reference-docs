# AID_IN

![AID_IN](./AID_IN.svg)

* * * * * * * * * *

## Einleitung

Der GlobalConstants-Baustein `AID_IN` definiert die Attribut-IDs eines **Eingabezahlenobjekts** (Input Number Object) im ISOBUS-UT-Kontext. Diese Konstanten werden verwendet, um auf spezifische Eigenschaften des Objekts zuzugreifen, wie z. B. Breite, Höhe, Farbwerte oder Formatierungsoptionen. Die Definition erfolgt gemäß dem Standard IEC 61499-1 und ist Teil des Pakets `isobus::UT::Q::const::AID`.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine. Der Baustein besitzt keine Ereignis-Eingänge, da er ausschließlich konstante Werte bereitstellt.

### **Ereignis-Ausgänge**

Keine. Es existieren keine Ereignis-Ausgänge.

### **Daten-Eingänge**

Keine. Der Baustein besitzt keine Daten-Eingänge, da die Werte als globale Konstanten fest definiert sind.

### **Daten-Ausgänge**

Keine. Es werden keine Datenwerte über Schnittstellen ausgegeben.

### **Adapter**

Keine. Es sind keine Adapter-Schnittstellen vorhanden.

## Funktionsweise

Der Baustein `AID_IN` stellt eine Sammlung von **15 Konstanten** vom Typ `USINT` (Unsigned Short Integer) bereit. Jede Konstante entspricht einer Attribut-ID, die im ISOBUS-Protokoll für ein Eingabezahlenobjekt verwendet wird. Die Werte ermöglichen den Zugriff auf folgende Attribute:

| Konstante          | Wert | Beschreibung                                                                 |
|--------------------|------|------------------------------------------------------------------------------|
| `WIDTH`            | 1    | Breite in Pixeln                                                            |
| `HEIGHT`           | 2    | Höhe in Pixeln                                                              |
| `BACKGROUND_COLOUR`| 3    | Index für die Hintergrundfarbe                                               |
| `FONT_ATT`         | 4    | Objekt-ID eines Font-Attributobjekts                                         |
| `OPTIONS`          | 5    | Bitmaske: Bit 0 = transparent, Bit 1 = führende Nullen anzeigen, Bit 2 = Null als leer anzeigen, Bit 3 = Dezimalstellen abschneiden (sonst runden) |
| `VARIABLE_REF`     | 6    | Objekt-ID eines Variablenobjekts                                            |
| `MIN_VALUE`        | 7    | Minimalwert                                                                 |
| `MAX_VALUE`        | 8    | Maximalwert                                                                 |
| `OFFSET`           | 9    | Offset                                                                      |
| `SCALE`            | 10   | Skalierungsfaktor                                                           |
| `NUMB_DECIMALS`    | 11   | Anzahl der Dezimalstellen                                                   |
| `FORMAT`           | 12   | Format: 0 = feste Dezimaldarstellung (####.nn), 1 = Exponentialformat        |
| `JUSTIFICATION`    | 13   | Ausrichtung: Bits 0–1 horizontal (0=links, 1=mittig, 2=rechts), Bits 2–3 vertikal (0=oben, 1=mittig, 2=unten) |
| `VALUE`            | 14   | Aktueller Wert des Objekts                                                  |
| `OPTIONS_2`        | 15   | Bitmaske: Bit 0 = aktiviert, Bit 1 = Echtzeit-Bearbeitung                     |

Diese Konstanten werden in Anwendungen genutzt, um Attribute eines Eingabezahlenobjekts zu setzen oder zu lesen, beispielsweise bei der Konfiguration eines Bedienterminals.

## Technische Besonderheiten

- **Datentyp**: Alle Konstanten sind vom Typ `USINT` (8‑Bit ohne Vorzeichen) und initial mit ihrem jeweiligen Attributwert belegt.
- **Paketzuordnung**: Der Baustein gehört zum Paket `isobus::UT::Q::const::AID`, was eine klare Strukturierung im Projekt ermöglicht.
- **Lizenz**: Die Definitionen stehen unter der **Eclipse Public License 2.0** (EPL-2.0). Eine Kopie der Lizenz ist unter [eclipse.org/legal/epl-2.0](https://www.eclipse.org/legal/epl-2.0/) verfügbar.
- **Standardkonformität**: Die Konstanten folgen dem IEC 61499-1-Standard für die Modellierung verteilter Automatisierungssysteme.

## Zustandsübersicht

Nicht anwendbar. Da es sich um eine reine Konstantendefinition handelt, existieren keine Zustände oder Zustandsübergänge.

## Anwendungsszenarien

- **Konfiguration eines ISOBUS-Bedienterminals**: Ein Programmierer verwendet `AID_IN.WIDTH`, um die Breite eines numerischen Eingabefelds zu definieren.
- **Dynamische Anpassung von Objekteigenschaften**: Bei der Laufzeit können Werte wie `MIN_VALUE` und `MAX_VALUE` referenziert werden, um Eingabevalidierungen durchzuführen.
- **Formatierung von Zahlen**: Über `FORMAT` und `NUMB_DECIMALS` kann die Darstellung von Dezimalzahlen gesteuert werden.
- **Zugriff auf Variablenwerte**: Über `VARIABLE_REF` kann eine Verknüpfung zu einer externen Variable hergestellt werden.

## Vergleich mit ähnlichen Bausteinen

Im ISOBUS-UT-Standard existieren analoge Konstantendefinitionen für andere Objekttypen, z. B. `AID_OUT` für Ausgabezahlenobjekte oder `AID_LABEL` für Beschriftungen. Gemeinsam ist allen die Struktur als `GLOBALCONSTANTS`, die die Attribut-IDs einheitlich und typsicher bereitstellen. Der Unterschied liegt in den spezifischen Attributen, die auf den jeweiligen Objekttyp zugeschnitten sind.

## Fazit

Der `AID_IN`-Baustein bietet eine klar definierte und standardisierte Sammlung von Konstanten für die Arbeit mit Eingabezahlenobjekten im ISOBUS-UT-Umfeld. Durch die zentrale Bereitstellung der Attribut-IDs wird die Programmierung erleichtert, Fehler durch magische Zahlen vermieden und die Wartbarkeit von Automatisierungsprojekten verbessert. Die vollständige Integration in das Paket- und Lizenzsystem der 4diac-IDE gewährleistet eine nahtlose Verwendung in verteilten Steuerungssystemen.

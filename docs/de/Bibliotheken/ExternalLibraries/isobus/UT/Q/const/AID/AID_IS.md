# AID_IS

![AID_IS](./AID_IS.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `AID_IS` ist ein globaler Konstanten-Baustein (GlobalConstants) im Kontext des ISO 11783 (ISOBUS) Standards. Er definiert numerische Konstanten, die als Attribut-IDs für das **Input String Object** verwendet werden. Diese IDs werden benötigt, um in der ISOBUS-Kommunikation auf bestimmte Eigenschaften von Textfeldern (z. B. Breite, Höhe, Hintergrundfarbe) zuzugreifen. Der Baustein stellt die Konstanten als `USINT`-Werte (0 bis 255) bereit und erleichtert so die lesbare Programmierung von ISOBUS-Objekten.

## Schnittstellenstruktur

Da es sich um einen globalen Konstanten-Baustein handelt, besitzt er keine klassischen Ein-/Ausgänge im Sinne eines Funktionsblocks. Die folgenden Abschnitte sind daher leer oder zeigen explizit "Keine".

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

Der Baustein definiert neun Konstanten, die jeweils eine bestimmte Attribut-ID für das ISOBUS-Input-String-Objekt repräsentieren. Diese Konstanten können in anderen Bausteinen referenziert werden, um die korrekte ID numerisch zu verwenden, ohne hartkodierte Zahlen im Code zu benötigen.

Die im Baustein enthaltenen Konstanten sind:

| Konstante            | Wert | Beschreibung                                                                                   |
|----------------------|------|------------------------------------------------------------------------------------------------|
| `WIDTH`              | 1    | Breite des Textfelds in Pixeln (AID_IS_WIDTH).                                                 |
| `HEIGHT`             | 2    | Höhe des Textfelds in Pixeln (AID_IS_HEIGHT).                                                  |
| `BACKGROUND_COLOUR`  | 3    | Farbindex des Hintergrunds (AID_IS_BACKGROUND_COLOUR).                                         |
| `FONT_ATT`           | 4    | Objekt-ID eines Font-Attributes-Objekts (AID_IS_FONT_ATT).                                     |
| `INP_ATT`            | 5    | Objekt-ID eines Input-Attributes- oder Extended-Input-Attributes-Objekts (AID_IS_INP_ATT).     |
| `OPTIONS`            | 6    | Bitmaske für Optionen, z. B. Umrahmung oder Sichtbarkeit (AID_IS_OPTIONS).                     |
| `VARIABLE_REF`       | 7    | Objekt-ID eines String-Variable-Objekts, das den Textinhalt bereitstellt (AID_IS_VARIABLE_REF). |
| `JUSTIFICATION`      | 8    | Ausrichtung des Textes – Bits 0–1 horizontal (0=links, 1=mittig, 2=rechts), Bits 2–3 vertikal (0=oben, 1=mittig, 2=unten). |
| `ENABLED`            | 9    | Status des Feldes – 0 = deaktiviert, 1 = aktiviert (AID_IS_ENABLED).                           |

Diese Konstanten werden als `USINT` mit den entsprechenden Initialwerten definiert. Sie sind über den Paketnamen `isobus::UT::Q::const::AID` referenzierbar.

## Technische Besonderheiten

- **Typ**: `GLOBALCONSTANTS` – der Baustein ist kein ausführbarer FB, sondern eine Sammlung von Konstanten.
- **Datentyp**: Alle Werte sind vom Typ `USINT` (8-Bit unsigned integer), passend zu den ISOBUS-Attribut-IDs.
- **Kompatibilität**: Der Baustein ist für die Verwendung im Rahmen des Eclipse 4diac-IDE und des IEC 61499-Modells konzipiert.
- **Lizenz**: Die Definition steht unter der Eclipse Public License 2.0 (EPL-2.0).
- **Namensraum**: Die Konstanten sind im Paket `isobus::UT::Q::const::AID` organisiert, was eine klare hierarchische Einordnung ermöglicht.

## Zustandsübersicht

Da es sich um einen reinen Konstanten-Baustein handelt, gibt es keine Zustände oder Zustandsübergänge. Der Baustein ist statisch und unveränderlich.

## Anwendungsszenarien

- **ISOBUS-Terminal-Applikationen**: Bei der Erstellung von Bedienoberflächen für landwirtschaftliche Maschinen müssen Attribute von Input-String-Objekten gesetzt oder gelesen werden. Die Konstanten können direkt in FB-Programmen verwendet werden, um die Attribut-IDs zu referenzieren.
- **Objekt-Pool-Konfiguration**: Beim Aufbau eines ISOBUS-Objektpools (e.g. per Array) helfen diese Konstanten, eindeutige und lesbare Bezeichnungen für die Attribut-IDs zu verwenden.
- **Entwicklung und Wartung**: Durch die zentrale Definition wird die Änderbarkeit von IDs erleichtert, falls der Standard angepasst wird oder Hersteller-spezifische Erweiterungen nötig sind.

## Vergleich mit ähnlichen Bausteinen

Im ISOBUS-Kontext gibt es weitere globale Konstanten-Bausteine, z. B. für andere Objekttypen (Object IDs, Arbeitsaufträge). Der Baustein `AID_IS` spezialisiert sich ausschließlich auf das Input-String-Objekt und ist daher gezielt für Textfelder einsetzbar. Andere Bausteine wie `AID_OBJ` oder `AID_FONT` decken weitere Attributbereiche ab. Der Vorteil von `AID_IS` liegt in der klaren Trennung und Benennung der IDs für Textattribute, wodurch der Programmcode verständlicher und weniger fehleranfällig wird.

## Fazit

`AID_IS` ist ein nützlicher Hilfsbaustein für alle Entwickler, die mit ISOBUS-Objekten und insbesondere mit Textfeldern arbeiten. Er bietet eine definierte, zentrale Sammlung von Attribut-IDs in einem standardisierten Format. Obwohl er selbst keine Funktionalität besitzt, vereinfacht er die Implementierung und Pflege von ISOBUS-Kommunikationslogiken erheblich und trägt zur Lesbarkeit und Wartbarkeit des Codes bei.

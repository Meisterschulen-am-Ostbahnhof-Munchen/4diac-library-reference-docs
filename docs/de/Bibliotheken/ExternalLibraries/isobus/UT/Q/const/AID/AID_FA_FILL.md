# AID_FA_FILL

![AID_FA_FILL](./AID_FA_FILL.svg)

* * * * * * * * * *
## Einleitung
Der Baustein **AID_FA_FILL** definiert die Attribut-IDs für das Füll-Attribut eines Objekts im ISOBUS (ISO 11783). Er stellt globale Konstanten bereit, die als Referenzwerte für Fülltyp, Farbe und Muster verwendet werden. Diese Konstanten sind Teil des Pakets `isobus::UT::Q::const::AID` und werden typischerweise bei der Erstellung oder Manipulation von Objekten mit Füll-Eigenschaften in ISOBUS-Terminals eingesetzt.

## Schnittstellenstruktur
Der Baustein besitzt weder Ereignis-Eingänge, Ereignis-Ausgänge, Daten-Eingänge, Daten-Ausgänge noch Adapter, da es sich um eine reine Konstantensammlung handelt. Die Werte werden direkt als globale Konstanten zur Verfügung gestellt und können an beliebiger Stelle im 4diac-Programm referenziert werden.

| Konstantenname | Datentyp | Initialwert | Beschreibung |
|----------------|----------|-------------|--------------|
| `FILL_TYPE`    | `USINT`  | `1`         | Kennung für die Art der Füllung: 0 = keine Füllung, 1 = mit Linienfarbe, 2 = mit spezifizierter Farbe, 3 = mit Muster. |
| `COLOUR`       | `USINT`  | `2`         | Index der verwendeten Farbe für die Füllung. |
| `PATTERN`      | `USINT`  | `3`         | Kennung für das Füllmuster. |

## Funktionsweise
Die Konstanten dienen als numerische Identifikatoren (Attribut-IDs) für das Füll-Attribut eines ISOBUS-Objekts. Bei der Kommunikation zwischen Terminal und Steuergerät werden diese Werte verwendet, um die Eigenschaften eines grafischen Objekts (z. B. eines Bereichs oder einer Form) zu setzen oder abzufragen. Der Zugriff erfolgt über die globalen Konstanten, die im 4diac-IDE als Konstantenbausteine in den Netzwerken referenziert werden können.

## Technische Besonderheiten
- Der Baustein ist als `GlobalConstants`-Element definiert und enthält ausschließlich Konstanten, keine ausführbare Logik.
- Die Konstanten sind im Compiler-Paket `isobus::UT::Q::const::AID` enthalten und werden in der Regel von anderen Funktionsbausteinen oder Adaptern importiert.
- Die Lizenzierung erfolgt unter der Eclipse Public License 2.0 (EPL-2.0), was die Weiterverwendung und Anpassung erlaubt.
- Die Werte sind als `USINT` (Unsigned Short Integer) deklariert und werden mit dem Präfix `USINT#` initialisiert.

## Zustandsübersicht
Da es sich um einen reinen Konstanten-Baustein handelt, existiert kein Zustandsmodell. Es gibt keine internen Zustände, Übergänge oder ereignisgesteuerte Abläufe.

## Anwendungsszenarien
- **ISOBUS-Objektprogrammierung:** Ein Funktionsbaustein, der ein grafisches Objekt mit Füllung erzeugt, verwendet `AID_FA_FILL.FILL_TYPE`, `AID_FA_FILL.COLOUR` und `AID_FA_FILL.PATTERN`, um die entsprechenden Attribut-IDs zu setzen.
- **Attributabfrage:** Bei der Auswertung von ISOBUS-Nachrichten können diese Konstanten verwendet werden, um zu prüfen, ob das erhaltene Attribut dem Fülltyp, der Farbe oder dem Muster entspricht.
- **Konfiguration von Terminals:** In einem übergeordneten Konfigurationswerkzeug können die Konstanten genutzt werden, um benutzerdefinierte Füllungen für Objekte festzulegen.

## Vergleich mit ähnlichen Bausteinen
Ähnliche Konstanten-Bausteine existieren für andere Objekteigenschaften, z. B. `AID_FA_LINE` für Linienattribute oder `AID_FA_LINE_STYLE` für Linienstile. Diese Bausteine folgen demselben Muster: Sie definieren numerische IDs als globale Konstanten und ermöglichen eine einheitliche Referenzierung im gesamten Programm. Der Unterschied liegt lediglich in den spezifischen Attributwerten und ihrer Semantik.

## Fazit
Der Baustein `AID_FA_FILL` ist eine einfache, aber wichtige Sammlung von Konstanten, die die Attribut-IDs für Füllungen in ISOBUS-konformen Anwendungen bereitstellt. Durch die klare Definition und die zentrale Bereitstellung als globale Konstanten wird die Wartbarkeit und Lesbarkeit von 4diac-Projekten verbessert. Er eignet sich ideal für die Entwicklung von Anwendungen im Bereich der landwirtschaftlichen Elektronik, die auf ISOBUS-Kommunikation basieren.
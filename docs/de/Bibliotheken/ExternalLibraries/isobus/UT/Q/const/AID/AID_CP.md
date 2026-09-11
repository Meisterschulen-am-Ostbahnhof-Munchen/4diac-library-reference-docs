# AID_CP

![AID_CP](./AID_CP.svg)

* * * * * * * * * *
## Einleitung
Der GlobalConstants-Baustein `AID_CP` definiert Konstanten für Attribut-IDs des Farbpaletten-Objekts im ISOBUS-Kontext. Er ist Teil des Pakets `isobus::UT::Q::const::AID`. Der Baustein stellt eine Konstante `OPTIONS` bereit, die eine bestimmte Attribut-ID identifiziert.

## Schnittstellenstruktur
Da es sich um einen GlobalConstants-Baustein handelt, besitzt er keine Ereignis-Eingänge, Ereignis-Ausgänge, Daten-Eingänge, Daten-Ausgänge oder Adapter. Stattdessen werden globale Konstanten definiert, die zur Compile-Zeit verfügbar sind.

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
Der Baustein stellt die globale Konstante `OPTIONS` vom Typ `USINT` mit dem Initialwert `USINT#1` bereit. Diese repräsentiert die Attribut-ID `AID_CP_OPTIONS` für das Farbpaletten-Objekt. Der Kommentar weist darauf hin, dass der Wert normalerweise 0 sein sollte – hier wird jedoch 1 verwendet, was vermutlich eine spezifische Konfiguration oder einen Platzhalter darstellt.

## Technische Besonderheiten
- Der Baustein ist als `GlobalConstants` deklariert, wodurch die Konstanten zur Übersetzungszeit aufgelöst werden und keine Laufzeitressourcen belegen.
- Die Konstante ist im Paket `isobus::UT::Q::const::AID` organisiert, was eine klare Strukturierung für ISOBUS-bezogene Konstanten bietet.
- Der Datentyp `USINT` (Unsigned Short Integer) hat eine Größe von 8 Bit.

## Zustandsübersicht
Nicht relevant, da der Baustein keine Zustandslogik oder Ausführungszustände besitzt.

## Anwendungsszenarien
- Bereitstellung standardisierter Attribut-IDs für ISOBUS-Kommunikation, insbesondere für Farbpaletten-Objekte.
- Verwendung in ISOBUS-Implementierungen zur Identifikation von Optionen oder Konfigurationen eines Farbpaletten-Objekts.
- Unterstützung der Wartbarkeit, indem magische Zahlen durch sprechende Namen ersetzt werden.

## Vergleich mit ähnlichen Bausteinen
Ähnliche GlobalConstants-Bausteine könnten andere Attribut-IDs für unterschiedliche ISOBUS-Objekte definieren (z.B. `AID_CPC` für Farbpaletten-Command, `AID_CPP` für Farbpaletten-Properties). Im Gegensatz zu Funktionsblöcken oder Adaptern besitzen diese Bausteine keine ausführbare Logik, sondern dienen ausschließlich der Konstantendefinition.

## Fazit
`AID_CP` ist ein einfacher GlobalConstants-Baustein zur Definition einer spezifischen Attribut-ID für ISOBUS-Farbpaletten. Er trägt zur Standardisierung und Strukturierung des Codes bei und erleichtert die Wartung durch die Verwendung benannter Konstanten.
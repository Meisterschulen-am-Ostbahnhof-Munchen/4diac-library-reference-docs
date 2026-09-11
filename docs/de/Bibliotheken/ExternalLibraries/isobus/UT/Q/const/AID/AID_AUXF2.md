# AID_AUXF2

![AID_AUXF2](./AID_AUXF2.svg)

* * * * * * * * * *
## Einleitung
Der Baustein **AID_AUXF2** ist ein GlobalConstants-Objekt der 4diac-IDE. Er definiert die Attribut-IDs für das **Auxiliary Function Type 2 (AUX-F2)**-Objekt im ISOBUS-Kontext. Diese Konstanten werden verwendet, um auf die Attribute **Hintergrundfarbe** und **Funktionstyp** eines solchen Objekts zuzugreifen. Der Baustein stellt die Werte als globale, unveränderliche Konstanten bereit und dient als zentraler Referenzpunkt für die entsprechenden Attribut-IDs.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
Keine. Dieser Baustein besitzt keine Ereignis-Eingänge, da es sich um ein reines Konstantenobjekt handelt.

### **Ereignis-Ausgänge**
Keine. Es werden keine Ereignisse ausgelöst oder weitergegeben.

### **Daten-Eingänge**
Keine. Die Konstanten sind fest definiert und benötigen keine Eingabedaten.

### **Daten-Ausgänge**
Dieser Baustein stellt folgende globale Konstanten bereit, die als Ausgabewerte interpretiert werden können:

| Name               | Datentyp | Wert  | Kommentar                                                                                  |
|--------------------|----------|-------|--------------------------------------------------------------------------------------------|
| `BACKGROUND_COLOUR`| `USINT`  | `1`   | AID_AUXF2_BACKGROUND_COLOUR – Index für die Hintergrundfarbe.                              |
| `FUNC`             | `USINT`  | `2`   | AID_AUXF2_FUNC – Bitmaske: Bits 0-4 = Funktionstyp (0-14), Bit 5 = Critical Control, Bit 6 = Assignment Restriction, Bit 7 = Single-assignment. |

### **Adapter**
Keine. Es sind keine Adapter definiert.

## Funktionsweise
Der Baustein *AID_AUXF2* ist ein reines Konstantenpaket. Er definiert über das Schlüsselwort `VAR_GLOBAL CONSTANT` zwei unveränderliche Werte vom Typ `USINT`. Diese Werte sind im gesamten Projekt verfügbar und können von anderen Funktionsbausteinen gelesen werden, um auf die entsprechenden Attribut-IDs des AUX-F2-Objekts zuzugreifen. Der Zugriff erfolgt über den Bausteinnamen, z.B. `AID_AUXF2.BACKGROUND_COLOUR`. Die Konstanten dienen der Lesbarkeit und Wartbarkeit des Codes, da sie symbolische Namen für magische Zahlen bereitstellen.

## Technische Besonderheiten
- Der Baustein ist als `GlobalConstants`-Objekt der Eclipse 4diac-IDE implementiert.
- Er trägt den `CompilerInfo`-Paketnamen `isobus::UT::Q::const::AID`, was auf eine organisierte Zuordnung innerhalb eines ISOBUS-Projekts hinweist.
- Die Konstanten sind als `USINT` (Unsigned Short Integer) definiert und haben feste Initialisierungswerte.
- Es gibt keinerlei Laufzeitlogik, Zustände oder dynamische Berechnungen.

## Zustandsübersicht
Da es sich um ein statisches Konstantenobjekt handelt, existiert kein Zustandsautomat. Der Zustand ist immer konstant und unveränderlich.

## Anwendungsszenarien
- **ISOBUS-Kommunikation:** Bei der Implementierung von ISOBUS-Nachrichten für AUX-Funktionen können diese Konstanten verwendet werden, um die Attribut-IDs für Hintergrundfarbe und Funktionstyp zu referenzieren.
- **Konfiguration von Bedienterminals:** In landwirtschaftlichen Maschinen können AUX-F2-Objekte auf Terminals konfiguriert werden; die Konstanten erleichtern die Programmierung dieser Konfiguration.
- **Vereinheitlichung von Werten:** Durch die zentrale Definition werden Tippfehler vermieden und spätere Änderungen der IDs zentral angepasst.

## Vergleich mit ähnlichen Bausteinen
Es existieren weitere ähnliche GlobalConstants-Objekte, z.B. `AID_AUXF1` oder `AID_AUXF3`, die sich auf andere AUX-Funktionszeiger beziehen. Diese Bausteine folgen demselben Muster: Sie definieren eine Liste von Attribut-IDs für das jeweilige AUX-Objekt. Der Unterschied liegt lediglich in den spezifischen Konstanten und deren Werten. `AID_AUXF2` ist speziell für den Typ 2 vorgesehen.

## Fazit
Der Baustein **AID_AUXF2** erfüllt eine wichtige Aufgabe in der ISOBUS-Entwicklung, indem er die Attribut-IDs für AUX-F2-Objekte standardisiert und als globale Konstanten bereitstellt. Durch seine einfache, rein deklarative Struktur ist er wartungsfreundlich und trägt zur Codequalität bei. Er ist ein typisches Beispiel für die Nutzung von GlobalConstants in der 4diac-IDE, um feste Werte sicher und zentral zu verwalten.
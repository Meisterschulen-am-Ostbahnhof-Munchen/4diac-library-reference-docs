# AID_OL

![AID_OL](./AID_OL.svg)

* * * * * * * * * *
## Einleitung

Der globale Konstantenblock **AID_OL** definiert die Attribut-IDs (Object Attribute IDs) für das **Output Line Object** (Ausgabe-Linienobjekt) im ISOBUS-Kontext. Diese Konstanten dienen als eindeutige Referenzen für die Identifikation spezifischer Attribute eines Linienobjekts, das in der isobus-Klasse `isobus::UT::Q` verwendet wird. Durch die Bereitstellung symbolischer Namen wird der Code lesbarer und weniger fehleranfällig, da numerische IDs nicht mehr direkt verwendet werden müssen.

## Schnittstellenstruktur

Da **AID_OL** ein globaler Konstantenblock und kein Funktionsblock, Adapter oder Subapp ist, besitzt er keine klassischen Schnittstellenelemente wie Ereignis- oder Datenports. Die bereitgestellten Konstanten sind als **globale Variablen** verfügbar und können vom gesamten Projekt referenziert werden.

### **Ereignis-Eingänge**
Keine (nicht vorhanden – reine Konstantendefinition).

### **Ereignis-Ausgänge**
Keine (nicht vorhanden – reine Konstantendefinition).

### **Daten-Eingänge**
Keine (nicht vorhanden – reine Konstantendefinition).

### **Daten-Ausgänge**
Keine (nicht vorhanden – reine Konstantendefinition).

### **Adapter**
Keine (nicht vorhanden – reine Konstantendefinition).

## Funktionsweise

Der Konstantenblock stellt vier Konstanten vom Typ `USINT` (Unsigned Short Integer) bereit, die jeweils eine bestimmte Attribut-ID eines Output-Line-Objekts repräsentieren. Die Werte sind fest definiert und können während der Laufzeit nicht verändert werden. Sie dienen als Parameter für Funktionen oder Dienstleistungen, die auf die Attribute eines Linienobjekts zugreifen (z.B. beim Setzen oder Abfragen von Eigenschaften). Durch die symbolischen Bezeichnungen wird die Nutzung im Quellcode erleichtert und die Lesbarkeit verbessert.

Die einzelnen Konstanten sind:

| Konstante | Wert (USINT) | Beschreibung |
|-----------|-------------|--------------|
| `LINE_ATT` | 1 | Objekt-ID eines Line-Attribut-Objekts (AID_OL_LINE_ATT) |
| `WIDTH`    | 2 | Breite des Linienobjekts in Pixeln (AID_OL_WIDTH) |
| `HEIGHT`   | 3 | Höhe des Linienobjekts in Pixeln (AID_OL_HEIGHT) |
| `LINE_DIR` | 4 | Linienrichtung: 0 = von links oben nach rechts unten, 1 = von links unten nach rechts oben (AID_OL_LINE_DIR) |

## Technische Besonderheiten

- **Datentyp:** Alle Konstanten sind als `USINT` (8-Bit unsigned integer) deklariert.
- **Wertebereich:** Die Werte sind von 1 bis 4 aufsteigend festgelegt.
- **Bedeutung:** Jede Zahl hat eine feste semantische Zuordnung im ISOBUS-Protokoll.
- **Initialwerte:** Die Werte sind als Konstanten initialisiert und können nicht überschrieben werden.
- **Verwendungszweck:** Sie werden typischerweise in der ISOBUS-Kommunikation verwendet, um Attribute eines Output-Line-Objekts zu identifizieren – beispielsweise bei der Konfiguration von virtuellen Terminals oder der Übertragung von Grafikdaten.

## Zustandsübersicht

Es existieren keine dynamischen Zustände, da der Block ausschließlich statische Konstanten definiert. Der Zustand ist immer identisch: die Werte sind fest und unveränderlich.

## Anwendungsszenarien

- **ISOBUS-Implementierung:** Verwendung in Anwendungen, die mit ISOBUS-kompatiblen Terminals oder Steuergeräten kommunizieren und Linienobjekte darstellen oder verwalten.
- **Objektbibliotheken:** Einbindung in Objektbibliotheken (Object Pool), um die Attribut-IDs des Output-Line-Objekts konsistent zu referenzieren.
- **Konfiguration:** Setzen von Linieneigenschaften (Breite, Höhe, Richtung) über die entsprechenden IDs.
- **Entwicklungsunterstützung:** Erhöhung der Code-Qualität durch sprechende Namen anstelle magischer Zahlen.

## Vergleich mit ähnlichen Bausteinen

Im ISOBUS-Umfeld existieren zahlreiche weitere Global-Konstantenblöcke für unterschiedliche Objekttypen (z.B. `AID_OL` für Output Line, `AID_AL` für Alarm, `AID_NM` für Number, etc.). Diese folgen demselben Muster: Sie definieren Attribut-IDs als benannte Konstanten und ermöglichen so eine einheitliche und fehlerresistente Programmierung. `AID_OL` ist speziell auf die Attribute des Output-Line-Objekts zugeschnitten.

## Fazit

Der globale Konstantenblock `AID_OL` stellt eine kompakte und klar strukturierte Sammlung von Attribut-IDs für ISOBUS-Ausgabe-Linienobjekte bereit. Durch die Verwendung sprechender Namen wird die Wartbarkeit und Lesbarkeit von ISOBUS-Anwendungen erheblich verbessert. Die konstante Definition verhindert versehentliche Änderungen und sorgt für eine stabile, protokollkonforme Implementierung.
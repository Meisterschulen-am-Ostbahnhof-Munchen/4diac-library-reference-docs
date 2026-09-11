# AID_LA

![AID_LA](./AID_LA.svg)

* * * * * * * * * *
## Einleitung

Der Baustein `AID_LA` ist ein Satz globaler Konstanten, der die Attribut-IDs für Linienobjekte (Line Attributes) im ISOBUS-Protokoll definiert. Diese Konstanten werden verwendet, um auf spezifische Eigenschaften von Linienobjekten zuzugreifen, beispielsweise Farbe, Breite und Art der Linie. Sie dienen als gemeinsame Referenz für andere Bausteine und Anwendungen, die mit Linien in der virtuellen Benutzeroberfläche arbeiten.

## Schnittstellenstruktur

Da es sich bei `AID_LA` um eine reine Konstantendefinition handelt, besitzt der Baustein keine Ereignis- oder Datenein-/ausgänge. Die bereitgestellten Informationen sind als globale Konstanten verfügbar und können direkt über die Namen referenziert werden.

### **Ereignis-Eingänge**

Nicht zutreffend – Es sind keine Ereignis-Eingänge definiert.

### **Ereignis-Ausgänge**

Nicht zutreffend – Es sind keine Ereignis-Ausgänge definiert.

### **Daten-Eingänge**

Nicht zutreffend – Es sind keine Daten-Eingänge definiert.

### **Daten-Ausgänge**

Nicht zutreffend – Es sind keine Daten-Ausgänge definiert.

### **Adapter**

Nicht zutreffend – Es sind keine Adapter definiert.

## Funktionsweise

Der Baustein stellt drei Konstanten vom Typ `USINT` (Unsigned Short Integer) bereit:

| Name          | Wert | Kommentar                                                                 |
|---------------|------|---------------------------------------------------------------------------|
| `LINE_COLOUR` | 1    | AID_LA_LINE_COLOUR – ID für die Linienfarbe                                |
| `LINE_WIDTH`  | 2    | AID_LA_LINE_WIDTH – ID für die Linienbreite                                |
| `LINE_ART`    | 3    | AID_LA_LINE_ART – Bitmuster für die Linienart (jedes Bit repräsentiert einen Pinselpunkt) |

Diese Konstanten werden typischerweise als Schlüsselwerte verwendet, um in Objektlisten auf die jeweilige Eigenschaft einer Linie zuzugreifen. Die Werte sind fest definiert und während der Laufzeit nicht veränderbar.

## Technische Besonderheiten

- **Datentyp:** Alle Konstanten sind als `USINT` deklariert, was einen Wertebereich von 0 bis 255 ermöglicht und im Speicher nur ein Byte belegt.
- **Initialwerte:** Die Werte sind mit `USINT#1`, `USINT#2` und `USINT#3` vorbelegt und werden beim Systemstart geladen.
- **Zugehörigkeit:** Der Baustein ist Teil des Pakets `isobus::UT::Q::const::AID` und wird über den Compiler als globale Konstantenquelle eingebunden.
- **Kommentar:** Der Kommentar gibt den Zweck der einzelnen Konstanten an und dient der Selbstbeschreibung im Quellcode.

## Zustandsübersicht

Da es sich um reine Datenkonstanten handelt, gibt es keinen Zustandsautomaten oder dynamisches Verhalten. Die Werte sind statisch und stehen ab der Initialisierung des Systems zur Verfügung.

## Anwendungsszenarien

- **ISOBUS-Implementierung:** Verwendung in Objekten, die Linienattribute darstellen, z.B. für den Zugriff auf die Linienfarbe oder -breite in einem grafischen Terminal.
- **Objektdefinition:** Als Referenz in Verbindung mit Objekt-IDs, um die Eigenschaften eines Linienobjekts eindeutig zu identifizieren.
- **Konfiguration:** Zur Parametrierung von Anzeigeelementen in landwirtschaftlichen Maschinensteuerungen, die auf dem ISOBUS-Standard basieren.

## Vergleich mit ähnlichen Bausteinen

Ähnliche Konstantendefinitionen existieren für andere Objektattribute (z.B. `AID_OBJ` für allgemeine Objekte oder `AID_OP` für Bedienelemente). Diese folgen demselben Muster, unterscheiden sich jedoch in der Anzahl und Bedeutung der definierten Konstanten. `AID_LA` fokussiert ausschließlich auf Linienattribute und bietet eine kompakte, klar abgegrenzte Menge von IDs, die leicht in andere Bausteine integriert werden können.

## Fazit

`AID_LA` ist eine einfache und dennoch essentielle Konstantendefinition für die Arbeit mit Linienobjekten im ISOBUS-Umfeld. Sie stellt eindeutige IDs bereit, die eine standardisierte Kommunikation und Konfiguration ermöglichen. Durch die Verwendung von `USINT`-Werten bleibt der Speicherbedarf minimal und die Kompatibilität zu anderen Systembausteinen gewährleistet. Die Struktur ist klar und erweiterbar, falls zukünftige Versionen weitere Linienattribute benötigen.
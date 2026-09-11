# AID_OR

![AID_OR](./AID_OR.svg)

* * * * * * * * * *
## Einleitung

Der Baustein **AID_OR** ist ein globaler Konstantencontainer, der die Attribut-IDs für ein **Output Rectangle**-Objekt im ISOBUS-Objektmodell definiert. Diese IDs werden verwendet, um auf die verschiedenen Eigenschaften eines rechteckigen Objekts (z. B. Linienattribute, Breite, Höhe) zuzugreifen. Der Baustein dient als zentrale Referenz für die Verwendung dieser Konstanten in anderen Funktionsbausteinen oder Adaptern.

## Schnittstellenstruktur

Da es sich um einen **GlobalConstants**-Container handelt, besitzt er keine herkömmlichen ereignis- oder datenbasierten Schnittstellen. Stattdessen werden die definierten Konstanten global bereitgestellt und können innerhalb des Projekts direkt verwendet werden.

### **Ereignis-Eingänge**
Nicht vorhanden.

### **Ereignis-Ausgänge**
Nicht vorhanden.

### **Daten-Eingänge**
Nicht vorhanden.

### **Daten-Ausgänge**
Nicht vorhanden.

### **Adapter**
Nicht vorhanden.

### **Globale Konstanten**
| Konstantenname  | Datentyp | Initialwert | Beschreibung |
|-----------------|----------|-------------|--------------|
| LINE_ATT        | USINT    | USINT#1     | Objekt-ID eines Linienattribut-Objekts (AID_OR_LINE_ATT). |
| WIDTH           | USINT    | USINT#2     | Breite in Pixeln (AID_OR_WIDTH). |
| HEIGHT          | USINT    | USINT#3     | Höhe in Pixeln (AID_OR_HEIGHT). |
| LINE_SUPPR      | USINT    | USINT#4     | Linienunterdrückung (AID_OR_LINE_SUPPR). |
| FILL_ATT        | USINT    | USINT#5     | Objekt-ID eines Füllattribut-Objekts (AID_OR_FILL_ATT). |

## Funktionsweise

Der Baustein `AID_OR` stellt fünf konstante USINT-Werte bereit, die den Attribut-IDs eines **Output Rectangle**-Objekts im ISOBUS entsprechen. Diese IDs werden typischerweise in der ISOBUS-Kommunikation verwendet, um auf einzelne Eigenschaften des Rechtecks zuzugreifen, z. B. um die Linienart, Breite, Höhe oder Füllung zu referenzieren. Die Konstanten sind als **Global Constants** deklariert, sodass sie in allen Funktionsbausteinen des Projekts ohne weitere Definition verwendet werden können.

## Technische Besonderheiten

- Die Werte sind als **USINT** (Unsigned Short Integer) definiert und liegen im Bereich von 1 bis 5.
- Der Baustein folgt dem **ISOBUS-Standard 61499-1** und ist in der **Eclipse Public License 2.0** lizenziert.
- Die Konstanten sind unveränderlich (`CONSTANT`) und stehen daher während der Laufzeit als feste Werte zur Verfügung.
- Die Verwendung als globaler Container ermöglicht eine zentrale und einheitliche Referenzierung von Attribut-IDs im gesamten ISOBUS-basierten Steuerungscode.

## Zustandsübersicht

Der Baustein besitzt keine internen Zustände, da er ausschließlich Konstanten definiert. Es gibt weder Zustandsübergänge noch eine Ablaufsteuerung.

## Anwendungsszenarien

- **ISOBUS‑Objektbeschreibung**: Verwendung bei der Definition und Parametrierung von Output‑Rectangle‑Objekten, z. B. für die Anzeige von Rechtecken auf einem Terminal.
- **Attributreferenzierung**: Zugriff auf die Breite, Höhe oder Linienattribute eines Rechtecks über die jeweiligen Konstanten.
- **Kommunikation mit ISOBUS‑Bedienterminals**: Der Baustein kann in Steuerungsfunktionen eingebunden werden, die ISOBUS‑Nachrichten mit Attribut‑IDs für Rechtecke erzeugen oder auswerten.

## Vergleich mit ähnlichen Bausteinen

Es gibt ähnliche GlobalConstants‑Container für andere Objektarten im ISOBUS, z. B. für Linienobjekte (`AID_LINE`) oder Textobjekte (`AID_TXT`). Im Gegensatz zu diesen definiert `AID_OR` spezifisch die Attribute eines Output‑Rechtecks. Die grundlegende Struktur (mehrere USINT‑Konstanten) ist vergleichbar, unterscheidet sich jedoch in den konkreten Attribut‑IDs und deren Bedeutung.

## Fazit

`AID_OR` ist ein einfacher, aber wichtiger Konstantenbaustein für die ISOBUS‑Entwicklung. Er ermöglicht eine definierte und konsistente Verwendung der Attribut‑IDs für Rechteck‑Objekte. Durch die Bereitstellung als globale Konstanten wird die Wartbarkeit des Codes verbessert und die Gefahr von Tippfehlern oder falschen Zahlenwerten reduziert. Der Baustein erfüllt damit eine unterstützende Funktion in ISOBUS‑basierten Steuerungssystemen.
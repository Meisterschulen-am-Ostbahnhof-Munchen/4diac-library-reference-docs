# AID_GC

![AID_GC](./AID_GC.svg)

* * * * * * * * * *
## Einleitung

Der globale Konstantenblock **AID_GC** definiert die Attribut-Identifikatoren für ein Grafikkontextobjekt im ISOBUS-Protokoll. Er stellt numerische Konstanten bereit, mit denen auf die verschiedenen Eigenschaften eines grafischen Kontexts (z. B. Viewport, Farben, Schriften) zugegriffen werden kann. Diese Konstanten werden im Bereich der Virtual Terminal (VT) Anwendungen verwendet, um Objektattribute eindeutig zu referenzieren.

## Schnittstellenstruktur

Da es sich um einen GlobalConstants-Block handelt, besitzt dieser keine ereignis- oder datenbasierten Ein-/Ausgänge im herkömmlichen Sinne. Stattdessen stellt er eine Sammlung globaler Konstanten bereit, die von anderen Bausteinen direkt referenziert werden können.

### **Ereignis-Eingänge**
Keine vorhanden.

### **Ereignis-Ausgänge**
Keine vorhanden.

### **Daten-Eingänge**
Keine vorhanden.

### **Daten-Ausgänge**
Keine vorhanden.

### **Adapter**
Keine vorhanden.

## Funktionsweise

Der Block definiert 17 ganzzahlige Konstanten (Typ `USINT`) mit festen Werten von 1 bis 17. Jede Konstante repräsentiert eine spezifische Attribut-ID eines Grafikkontextobjekts. Die Namen sind sprechend gewählt und geben die Bedeutung der jeweiligen ID an, z. B. `VP_WIDTH` für die Breite des Viewports, `FG_COLOUR` für die Vordergrundfarbe oder `FORMAT` für den Zeichensatztypen. Diese Konstanten können in Applikationen verwendet werden, um auf Attribute von Grafikkontexten zuzugreifen, ohne die numerischen Werte direkt im Code zu verwenden.

## Technische Besonderheiten

- Alle Konstanten sind vom Typ `USINT` (8-Bit unsigned integer) und werden mit expliziten Initialwerten (`USINT#1` etc.) definiert.
- Die Kommentare zu den Konstanten enthalten zusätzliche Erläuterungen, z. B. für `FORMAT` die Bedeutung des Werts (0 = Monochrom, 1 = 4-Bit Farbe, 2 = 8-Bit Farbe) und für `OPTIONS` die Bitmaske zur Steuerung von Transparenz und Farbmodus.
- Der Block ist als `GlobalConstants` gekennzeichnet und kann in 4diac IDE direkt eingebunden werden, um die Konstanten projektweit verfügbar zu machen.

## Zustandsübersicht

Da es sich nicht um einen Funktionsblock mit Zustandslogik handelt, entfällt eine Zustandsübersicht.

## Anwendungsszenarien

- **ISOBUS-VT-Anwendungen:** Verwendung der Konstanten zur Konfiguration von Grafikobjekten (z. B. Setzen von Viewport-Parametern, Farben oder Schriftattributen).
- **Einheitliche Referenzierung:** Vermeidung von „magic numbers“ im Code durch Verwendung sprechender Konstantennamen.
- **Projektübergreifende Wiederverwendung:** Die globalen Konstanten können in mehreren Funktionsblöcken desselben Projekts referenziert werden, um Konsistenz zu gewährleisten.

## Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einem klassischen Funktionsblock (FB) besitzt der GlobalConstants-Block **keine** Ein-/Ausgangsschnittstellen und **keine** interne Verarbeitungslogik. Er dient ausschließlich der Definition von globalen Konstanten. Ähnliche Blöcke könnten z. B. `AID_OBJECT` oder `AID_VT` sein, die ebenfalls Konstanten für andere Objekttypen bereitstellen. Der Vorteil gegenüber einer direkten Definition im Code liegt in der zentralen und typisierten Verwaltung, die eine bessere Wartbarkeit und Übersichtlichkeit ermöglicht.

## Fazit

Der GlobalConstants-Block `AID_GC` ist eine wichtige Grundlage für die Entwicklung von ISOBUS-Terminal-Anwendungen in der 4diac-Umgebung. Er stellt eine klar strukturierte Sammlung von Attribut-IDs für Grafikkontexte bereit und erleichtert die Implementierung von VT-basierten HMI-Lösungen durch die Verwendung definierter Konstanten.
# Industry_Groups

![Industry_Groups](./Industry_Groups.svg)

* * * * * * * * * *
## Einleitung

Dieses Dokument beschreibt die globalen Konstanten für ISOBUS Industry Groups. Die Konstanten definieren die verschiedenen Industriegruppen (Industry Groups) im ISOBUS-Protokoll zur Klassifizierung von Geräten und Anwendungen.

## Schnittstellenstruktur

Da es sich um eine Sammlung globaler Konstanten handelt, gibt es keine Ereignis- oder Datenein-/ausgänge im Sinne eines Funktionsblocks.

### **Ereignis-Eingänge**

Nicht anwendbar.

### **Ereignis-Ausgänge**

Nicht anwendbar.

### **Daten-Eingänge**

Nicht anwendbar.

### **Daten-Ausgänge**

Nicht anwendbar.

### **Adapter**

Nicht anwendbar.

## Funktionsweise

Die Konstanten sind als Byte-Werte definiert und dienen zur Kennzeichnung der Industriegruppe in der ISOBUS-Kommunikation. Sie können in Anwendungen verwendet werden, um Geräteklassen zu identifizieren oder zu filtern.

## Technische Besonderheiten

- Alle Konstanten sind vom Typ `BYTE`.
- Die Werte reichen von 0 bis 7, wobei `0` für global und `1-5` für spezifische Gruppen stehen, und `6-7` für zukünftige Verwendung durch SAE reserviert sind.
- Die Konstanten sind als `CONSTANT` deklariert, d.h. sie sind unveränderlich und global verfügbar.

## Zustandsübersicht

Nicht zutreffend, da es sich um Konstanten handelt und keine Zustandsautomaten vorhanden sind.

## Anwendungsszenarien

- In ISOBUS-Netzwerken zur Identifikation der Geräteklasse (z.B. landwirtschaftliche Maschinen, Bauausrüstung, Marineanwendungen).
- Bei der Entwicklung von Steuerungsanwendungen, die auf die Industriegruppe reagieren müssen.
- In der Diagnose und Überwachung von ISOBUS-Kommunikation.

## Vergleich mit ähnlichen Bausteinen

Es gibt keine direkten ähnlichen Funktionsblöcke, jedoch bieten einige Bibliotheken vergleichbare Konstantendefinitionen für andere Protokolle (z.B. CAN-IDs). Diese Konstanten sind spezifisch für ISOBUS.

## Fazit

Die GlobalConstants `Industry_Groups` stellen eine klare und standardisierte Möglichkeit dar, Industriegruppen im ISOBUS-Kontext zu referenzieren. Sie sind einfach zu verwenden und tragen zur Interoperabilität bei.
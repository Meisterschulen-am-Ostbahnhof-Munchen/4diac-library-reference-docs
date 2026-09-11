# Manufacturer_IDs

![Manufacturer_IDs](./Manufacturer_IDs.svg)

* * * * * * * * * *

## Einleitung

Dieser GlobalConstants-Baustein definiert eine umfangreiche Liste von Hersteller-Identifikationsnummern (Manufacturer IDs) für den ISOBUS-Standard. Jede Konstante repräsentiert einen eindeutigen numerischen Wert, der einem bestimmten Hersteller zugeordnet ist. Diese IDs werden verwendet, um Geräte in landwirtschaftlichen und nutzfahrzeugbezogenen Netzwerken eindeutig zu identifizieren.

## Schnittstellenstruktur

Da es sich um einen GlobalConstants-Baustein handelt, besitzt er keine klassischen Ein- oder Ausgänge. Alle Werte sind als konstante Daten verfügbar.

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Die folgenden Konstanten sind als Daten-Ausgänge im Sinne von globalen Variablen verfügbar (Auszug):

- `M_FOR_EXPERIMENTAL_OR_DEVELOPMENTAL_USE_ONLY` (UINT, Wert 0)
- `M_BENDIX_COMMERCIAL_VEHICLE_SYSTEMS` (UINT, Wert 1)
- `M_ALLISON_TRANSMISSION` (UINT, Wert 2)
- ... (weitere Einträge bis `M_THOMAS_G_FARIA` mit Wert 1863)

Eine vollständige Liste befindet sich in der XML-Definition.

### **Adapter**

Keine.

## Funktionsweise

Die Konstanten sind als globale Konstanten definiert und stehen dem gesamten IEC-61499-Anwendungsmodell zur Verfügung. Sie können direkt referenziert werden, z.B. um die Hersteller-ID eines ISOBUS-Geräts zu setzen oder zu interpretieren. Die Werte sind hartcodiert und unveränderlich (CONSTANT).

## Technische Besonderheiten

- Die Konstanten sind vom Typ `UINT` (vorzeichenloser 16-Bit-Integer).
- Der Gültigkeitsbereich umfasst historisch vergebene IDs sowie reservierte und unbenutzte Nummern.
- Einige Einträge verweisen auf frühere Herstellernamen oder sind als Duplikate gekennzeichnet.
- Die Liste basiert auf der offiziellen ISOBUS-Listenverwaltung und wird stetig erweitert.

## Zustandsübersicht

Da es sich um Konstanten handelt, existiert kein Zustandsautomaten. Der Baustein ist passiv und liefert nur Werte.

## Anwendungsszenarien

- Identifikation des Herstellers eines angeschlossenen ISOBUS-Geräts.
- Festlegung der eigenen Hersteller-ID bei der Entwicklung von ISOBUS-kompatiblen Steuerungen.
- Verwendung in Diagnose- und Konfigurationstools zur Anzeige von Herstellerinformationen.

## Vergleich mit ähnlichen Bausteinen

Es gibt keine direkten Funktionsblöcke, die diese Konstanten ersetzen. Andere GlobalConstants-Bausteine könnten ähnliche Nummernkreise (z.B. für Protokoll-IDs oder Fehlercodes) definieren, jedoch ist dieser speziell auf Hersteller-IDs ausgerichtet.

## Fazit

Der GlobalConstants-Baustein `Manufacturer_IDs` stellt eine zentrale, umfassende Referenz für ISOBUS-Hersteller-IDs bereit. Er ist ein nützliches Hilfsmittel für Entwickler und Systemintegratoren, um herstellerspezifische Zuordnungen zu implementieren und zu verwalten. Seine Werte sind fest definiert und daher zuverlässig für den Einsatz in der Praxis.

---

Hinweis: Die vollständige Liste der Konstanten mit über 1600 Einträgen ist in der XML enthalten und kann bei Bedarf als Tabelle dargestellt werden. In dieser Dokumentation wurde nur ein Auszug gezeigt.

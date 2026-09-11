# zaehlstand

![zaehlstand](./zaehlstand.svg)

* * * * * * * * * *
## Einleitung
Der globale Konstantenblock `zaehlstand` definiert sieben symbolische SINT-Konstanten, die den aktuellen Zählstand laufender Motoren in einer Anlagensequenz repräsentieren. Die Werte reichen von 0 bis 6 und entsprechen den Zuständen von „kein Motor aktiv" bis „alle Motoren aktiv". Diese Konstanten dienen als einheitliche Referenz für Zustandsprüfungen und Vergleiche in der Steuerungslogik und erhöhen die Lesbarkeit sowie Wartbarkeit des IEC-61499-Codes.

## Schnittstellenstruktur
Der Baustein stellt keine klassischen Schnittstellen im Sinne von Funktionsbausteinen (Ereignis-Eingänge, Ereignis-Ausgänge, Daten-Eingänge, Daten-Ausgänge oder Adapter) bereit. Stattdessen definiert er globale Konstanten, die im gesamten Projekt als unveränderliche Werte verwendet werden können. Die folgende Tabelle listet die verfügbaren Konstanten auf:

| Konstantenbezeichner | Typ | Wert | Bedeutung |
|----------------------|-----|------|-----------|
| `ZAEHLSTAND_0`       | SINT| SINT#0 | 0 Motoren laufen (S_AUS) |
| `ZAEHLSTAND_1`       | SINT| SINT#1 | 1 Motor läuft |
| `ZAEHLSTAND_2`       | SINT| SINT#2 | 2 Motoren laufen |
| `ZAEHLSTAND_3`       | SINT| SINT#3 | 3 Motoren laufen |
| `ZAEHLSTAND_4`       | SINT| SINT#4 | 4 Motoren laufen |
| `ZAEHLSTAND_5`       | SINT| SINT#5 | 5 Motoren laufen |
| `ZAEHLSTAND_6`       | SINT| SINT#6 | 6 Motoren laufen (S_LAEUFT, alle) |

## Funktionsweise
Die Konstanten werden als globale Definitionen bereitgestellt und können in beliebigen Algorithmen oder Zustandsautomaten referenziert werden. Sie stellen feste, unveränderliche Werte dar, die den jeweiligen Motorzählstand eindeutig kennzeichnen. Die Verwendung symbolischer Namen anstelle magischer Zahlen verbessert die Codequalität und reduziert Fehler bei der Programmierung.

## Technische Besonderheiten
- **Gültigkeitsbereich**: Global, d.h. im gesamten Projekt verfügbar.
- **Datentyp**: Alle Konstanten sind vom Typ `SINT` (signed short integer) und belegen 8 Bit.
- **Initialwerte**: Die Werte sind als `SINT#0` bis `SINT#6` festgelegt und können nicht verändert werden.
- **Paketstruktur**: Die Konstanten sind im Paket `logiBUS::utils::sequence::const` definiert und folgen der IEC-61499-Spezifikation (Standard 61499-1).
- **Lizenz**: Die Definitionen stehen unter der Eclipse Public License 2.0 (EPL-2.0).

## Zustandsübersicht
Obwohl der Block selbst keine Zustandsmaschine implementiert, entsprechen die sieben definierten Konstanten den möglichen Zuständen der Motorenanzahl:
- `ZAEHLSTAND_0` – Zustand „alle Motoren aus" (S_AUS)
- `ZAEHLSTAND_1` bis `ZAEHLSTAND_5` – Zwischenzustände mit 1 bis 5 aktiven Motoren
- `ZAEHLSTAND_6` – Zustand „alle Motoren laufen" (S_LAEUFT)

Diese Zustände können in einer übergeordneten Steuerungslogik verwendet werden, um den aktuellen Betriebszustand der Anlage zu bestimmen.

## Anwendungsszenarien
- **Überwachung der Motoranzahl**: Ein Funktionsbaustein, der die Anzahl laufender Motoren zählt, kann die ermittelten Werte mit den Konstanten vergleichen und entsprechende Aktionen auslösen.
- **Zustandssteuerung**: In einer Anlagensteuerung dienen die Konstanten als Referenz für Zustandsübergänge, z.B. für die Aktivierung von Sicherheitsfunktionen oder Alarmen bei bestimmten Motorzahlen.
- **Vereinheitlichung**: Durch die zentrale Definition werden konsistente Bezeichnungen über verschiedene Module hinweg sichergestellt und Änderungen an Werten können zentral vorgenommen werden.

## Vergleich mit ähnlichen Bausteinen
Anders als ein Funktionsbaustein, der dynamische Daten verarbeitet, bietet dieser Block keinerlei Verarbeitungslogik. Er stellt ausschließlich statische Konstanten bereit. Vergleicht man ihn mit einem Aufzählungstyp (ENUM), so geht er über diesen hinaus, da er explizite SINT-Werte definiert und so auch als Eingabe für systemnahe Funktionen verwendet werden kann. Im Gegensatz zu einer Variablen, die zur Laufzeit geändert werden kann, sind diese Werte final und unveränderlich.

## Fazit
Der globale Konstantenblock `zaehlstand` ist ein einfaches, aber nützliches Hilfsmittel zur Definition von Motoranzahlzuständen in Anlagensequenzen. Er verbessert die Lesbarkeit des Steuercodes, vermeidet magische Zahlen und bietet eine zentrale und konsistente Basis für Zustandsprüfungen. Die klare Struktur und die IEC-61499-Konformität machen ihn zu einem sinnvollen Bestandteil der `logiBUS::utils`-Bibliothek.
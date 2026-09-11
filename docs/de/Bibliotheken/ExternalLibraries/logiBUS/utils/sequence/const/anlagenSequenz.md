# anlagenSequenz

![anlagenSequenz](./anlagenSequenz.svg)

* * * * * * * * * *
## Einleitung

Dieses Element der 4diac-IDE definiert eine Sammlung globaler Konstanten für die Steuerung einer Anlagen-Sequenz („AnlagenSequenz_06“). Die Konstanten beschreiben den aktuellen Betriebs- und Störungsstatus sowie die Art von Übergängen (Vorlauf/Nachlauf) für eine Anlage mit sechs Motoren. Sie werden im Paket `logiBUS::utils::sequence::const` bereitgestellt und können von verschiedenen Funktionsbausteinen der Anlagensequenz referenziert werden, um eine einheitliche und lesbare Codierung zu gewährleisten.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine (dies ist ein Global-Constants-Baustein, keine Ereignisse vorhanden).

### **Ereignis-Ausgänge**

Keine (dies ist ein Global-Constants-Baustein, keine Ereignisse vorhanden).

### **Daten-Eingänge**

Keine (die Werte sind statisch definiert und können nicht zur Laufzeit geändert werden).

### **Daten-Ausgänge**

Die folgenden globalen Konstanten sind als Daten-Ausgänge verfügbar (sie können in anderen Bausteinen gelesen werden):

| Konstante | Datentyp | Wert | Beschreibung |
|-----------|----------|------|--------------|
| `STATUS_BETRIEB_AUS` | SINT | SINT#0 | Anlage steht |
| `STATUS_BETRIEB_HOCHFAHREN` | SINT | SINT#1 | Vorlauf-Kette aktiv |
| `STATUS_BETRIEB_LAEUFT` | SINT | SINT#2 | alle 6 Motoren laufen |
| `STATUS_BETRIEB_HERUNTERFAHREN` | SINT | SINT#3 | Nachlauf-Kette aktiv |
| `STATUS_STOERUNG_KEINE` | SINT | SINT#0 | keine aktive Störung |
| `STATUS_STOERUNG_AKTIV` | SINT | SINT#4 | Störung aktiv, verriegelt bis EIN |
| `TRANSIT_ART_KEINE` | SINT | SINT#0 | kein Motor aktuell im Vor-/Nachlauf (S_AUS, S_LAEUFT) |
| `TRANSIT_ART_VORLAUF` | SINT | SINT#1 | TRANSIT_MOTOR startet nach Ablauf der aktuellen Zeit |
| `TRANSIT_ART_NACHLAUF` | SINT | SINT#2 | TRANSIT_MOTOR stoppt nach Ablauf der aktuellen Zeit |

### **Adapter**

Keine (es werden keine Adapter verwendet).

## Funktionsweise

Die globalen Konstanten dienen als zentrale Definitionsquelle für Zustandswerte in der Anlagensteuerung. Sie werden in der 4diac-IDE als `GlobalConstants` definiert und können in allen FB-Typen der Anwendung über ihren symbolischen Namen verwendet werden. Dadurch wird die Verwendung von „magic numbers“ vermieden und die Wartbarkeit des Steuerungscodes erhöht.

Die Konstanten sind in zwei Gruppen aufgeteilt:
- **Betriebszustände** (`STATUS_BETRIEB_*`): beschreiben die Phasen der Anlage (aus, hochfahren, laufen, herunterfahren).
- **Störungszustände** (`STATUS_STOERUNG_*`): geben an, ob eine Störung vorliegt.
- **Transitionsarten** (`TRANSIT_ART_*`): kennzeichnen die Art eines Motor-Übergangs (kein Übergang, Vorlauf, Nachlauf).

Durch die getrennten Konstanten für Betrieb und Störung wird eine klare Trennung der Zustandslogik ermöglicht.

## Technische Besonderheiten

- Die Konstanten sind als `SINT` (Small Integer) deklariert und mit dem Präfix `SINT#` initialisiert.
- Sie sind als `CONSTANT` in `VAR_GLOBAL CONSTANT` definiert, d.h. ihre Werte können während der Laufzeit nicht verändert werden.
- Der Baustein ist im Paket `logiBUS::utils::sequence::const` enthalten und hat die Versionsinformation `1.0` vom 12.08.2026.
- Die Konstanten sind direkt über den qualifizierten Namen ansprechbar, z.B. `anlagenSequenz.STATUS_BETRIEB_LAEUFT`.
- Eine Besonderheit ist, dass `STATUS_STOERUNG_AKTIV` den Wert `4` hat, was nicht in die numerische Reihenfolge der Betriebszustände passt. Dies ermöglicht eine unabhängige Behandlung von Störungen.

## Zustandsübersicht

Da es sich um eine Konstantendefinition handelt, gibt es keine internen Zustände. Die definierten Werte repräsentieren jedoch die möglichen Zustände der Anlage:

- **Betriebszustände**: `STATUS_BETRIEB_AUS` (0), `STATUS_BETRIEB_HOCHFAHREN` (1), `STATUS_BETRIEB_LAEUFT` (2), `STATUS_BETRIEB_HERUNTERFAHREN` (3).
- **Störungszustand**: `STATUS_STOERUNG_KEINE` (0), `STATUS_STOERUNG_AKTIV` (4).
- **Transitionsarten**: `TRANSIT_ART_KEINE` (0), `TRANSIT_ART_VORLAUF` (1), `TRANSIT_ART_NACHLAUF` (2).

Diese Zustände werden typischerweise in einer übergeordneten Anlagen-Sequenz-Steuerung verwendet, um den aktuellen Betriebszustand zu kennzeichnen und Übergänge zu steuern.

## Anwendungsszenarien

Die Konstanten werden in Funktionsbausteinen verwendet, die die Anlagen-Sequenz realisieren, z.B.:

- **Hochfahren**: Der Baustein setzt den Betriebszustand auf `STATUS_BETRIEB_HOCHFAHREN` und steuert die Motoren nacheinander an. Während dieser Phase ist die Transitionsart `TRANSIT_ART_VORLAUF`.
- **Nachlauf**: Beim Herunterfahren wird `STATUS_BETRIEB_HERUNTERFAHREN` und `TRANSIT_ART_NACHLAUF` verwendet.
- **Störungsbehandlung**: Tritt eine Störung auf, wird `STATUS_STOERUNG_AKTIV` gesetzt, und die Anlage wird bis zum nächsten EIN-Signal verriegelt.
- **Zustandsanzeige**: Über die Konstanten können Statuswerte einfach an HMI- oder Diagnosesysteme weitergegeben werden.

## Vergleich mit ähnlichen Bausteinen

Da es sich um eine reine Konstantendefinition handelt, gibt es keinen direkten Funktionsblock mit gleicher Aufgabe. Andere Bausteine könnten ähnliche Konstantenbereiche über `LocalConstants` oder `Directly Represented`-Variablen definieren, aber die Verwendung eines globalen Konstantenblocks wie `anlagenSequenz` bietet den Vorteil, dass alle zugehörigen Werte an einem zentralen Ort gepflegt werden können und für das gesamte Projekt einheitlich verfügbar sind.

## Fazit

Das `GlobalConstants`-Element `anlagenSequenz` stellt eine klar strukturierte Sammlung von Konstanten für die Zustands- und Übergangssteuerung einer Anlagen-Sequenz bereit. Es verbessert die Lesbarkeit und Wartbarkeit des IEC-61499-Codes, indem es symbolische Namen anstelle von Zahlenwerten verwendet. Die klare Trennung zwischen Betriebs- und Störungszuständen sowie die Definition von Transitionsarten ermöglicht eine robuste Implementierung der Steuerungslogik. Dieses Modul ist ein sinnvoller Bestandteil einer größeren 4diac-Lösung für die industrielle Anlagensteuerung.
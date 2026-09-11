# AUDI_RampLimitFS

![AUDI_RampLimitFS](./AUDI_RampLimitFS.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AUDI_RampLimitFS** ist ein gekapselter Rampenbegrenzer, der auf dem Basisbaustein `RampLimitFS` aufbaut und eine anwendungsorientierte Schnittstelle über AUDI- und AX-Adapter bereitstellt. Er ermöglicht das schrittweise Anfahren an einen Grenzwert („Hochrampe") und das schrittweise Verlassen („Rampen abwärts"), jeweils mit unterschiedlichen Rampenraten. Integrierte Statuslatch-Ausgänge melden, sobald der Ausgangswert die untere oder obere Grenze erreicht hat. Durch die Nutzung standardisierter Adapter vereinfacht der Baustein die Einbindung in bestehende Signalverarbeitungsketten.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Kommentar |
|----------|-----------|
| `INIT` | Initialisierungsanforderung; übernimmt die Parameter und startet den Baustein. (Mit `VAL_ZERO`, `SLOW`, `FAST`, `VAL_FULL`) |
| `ZERO` | Sprung auf den Minimalwert (`VAL_ZERO`). |
| `DOWN_FAST` | Rampenabwärts mit schneller Rate (`FAST`). |
| `DOWN_SLOW` | Rampenabwärts mit langsamer Rate (`SLOW`). |
| `UP_SLOW` | Rampenaufwärts mit langsamer Rate (`SLOW`). |
| `UP_FAST` | Rampenaufwärts mit schneller Rate (`FAST`). |
| `FULL` | Sprung auf den Maximalwert (`VAL_FULL`). |

### **Ereignis-Ausgänge**

| Ereignis | Kommentar |
|----------|-----------|
| `INITO` | Bestätigung der erfolgreichen Initialisierung. |

### **Daten-Eingänge**

| Variable | Typ | Initialwert | Kommentar |
|----------|-----|-------------|-----------|
| `VAL_ZERO` | DINT | 0 | Minimalwert der Rampe. |
| `SLOW` | DINT | 1 | Schrittweite für langsame Rampenbewegung pro Zyklus. |
| `FAST` | DINT | 10 | Schrittweite für schnelle Rampenbewegung pro Zyklus. |
| `VAL_FULL` | DINT | 100 | Maximalwert der Rampe. |

### **Daten-Ausgänge**

Direkte Datenausgänge sind nicht vorhanden. Alle Ausgangswerte werden über die **Adapter-Plugs** `OUT`, `qAtZero` und `qAtFull` bereitgestellt.

### **Adapter**

| Adapter | Richtung | Typ | Kommentar |
|---------|----------|-----|-----------|
| `LOAD` | Socket | `adapter::types::unidirectional::AUDI` | Empfängt das Zielwert-Signal (`PV`) und das zugehörige Ereignis (z.B. `E1`). |
| `OUT` | Plug | `adapter::types::unidirectional::AUDI` | Gibt den gerammpten Ausgangswert aus. |
| `qAtZero` | Plug | `adapter::types::unidirectional::AX` | Meldet (gelatcht), dass der Ausgang den Minimalwert erreicht hat. |
| `qAtFull` | Plug | `adapter::types::unidirectional::AX` | Meldet (gelatcht), dass der Ausgang den Maximalwert erreicht hat. |

## Funktionsweise

Der Baustein verarbeitet einen Zielwert, der über den `LOAD`-Adapter eintrifft, und erzeugt daraus einen rampenförmigen Ausgangswert, der über den `OUT`-Adapter ausgegeben wird. Der interne Kern `RampLimitFS` führt die eigentliche Rampenberechnung durch.  

Über die Ereigniseingänge `ZERO`, `FULL`, `DOWN_FAST`, `DOWN_SLOW`, `UP_SLOW` und `UP_FAST` wird die Richtung und Geschwindigkeit der Rampe gesteuert. Die Werte `VAL_ZERO` und `VAL_FULL` definieren die Unter- bzw. Obergrenze. Die Schrittweiten `SLOW` und `FAST` bestimmen, wie stark sich der Ausgangswert pro Ereigniszyklus ändert.  

Die Konvertierung zwischen `UDINT` (im AUDI-Adapter) und `DINT` (intern verwendeter Datentyp) wird durch die eingebetteten Funktionsbausteine `F_UDINT_TO_DINT` bzw. `F_DINT_TO_UDINT` automatisch durchgeführt.  

Die beiden Statusausgänge `qAtZero` und `qAtFull` werden durch zwei `E_D_FF`-Latchbausteine realisiert. Sobald der interne Rampenbaustein meldet, dass der Ausgang den Minimalwert (bzw. Maximalwert) erreicht hat, wird das entsprechende Latch gesetzt und anschließend über den AX-Adapter signalisiert. Die Latches bleiben solange gesetzt, bis sie durch eine neue Bedingung zurückgesetzt werden.

## Technische Besonderheiten

- **Adapterintegration**: Nutzung des unidirektionalen AUDI-Adapters für Werte und Ereignisse sowie des AX-Adapters für Statusinhalte. Dadurch ist eine modulare Einbindung in bestehende Signalpfade ohne proprietäre Schnittstellen möglich.
- **Datentypkonvertierung**: Automatische Umwandlung von `UDINT` (aus dem AUDI-Socket) in `DINT` für die interne Verarbeitung und anschließende Rückumwandlung für den AUDI-Plug.
- **Status-Latching**: Die Erreichung von Unter- und Obergrenze wird in Flip-Flops (`E_D_FF`) festgehalten, sodass auch kurze Zustandswechsel zuverlässig erfasst werden.
- **Mehrere Rampenraten**: Unterstützung von zwei unterschiedlichen Geschwindigkeiten (`SLOW` und `FAST`) ermöglicht feiner abgestufte positive und negative Bewegung.
- **Direkter Sprung** auf die Grenzwerte über die Ereignisse `ZERO` und `FULL`, unabhängig von Rampenzeiten.

## Zustandsübersicht

Der Baustein besitzt keine explizit sichtbaren Zustände, jedoch lässt sich der interne Ablauf wie folgt beschreiben:

- **Initialisierung**: Nach `INIT` werden die Grenzen und Schrittweiten übernommen; der Ausgang wird auf `VAL_ZERO` gesetzt (Standardverhalten laut Beschreibung).
- **Betrieb**: Abhängig von den anliegenden Ereignissen wird der Ausgangswert schrittweise erhöht (`UP_*`) oder verringert (`DOWN_*`), wobei die Schrittweite der jeweiligen Geschwindigkeit entspricht.
- **Grenzerreichen**: Wird die untere oder obere Grenze erreicht, aktiviert der Baustein die entsprechenden Statuslatch (`qAtZero`, `qAtFull`).
- **Sprünge**: Die Ereignisse `ZERO` und `FULL` setzen den Ausgang sofort auf den jeweiligen Grenzwert (ohne Rampenbewegung).

## Anwendungsszenarien

- **Antriebssteuerung**: Sanftes Hoch- und Herunterfahren von Förderbändern, Lüftern oder Pumpen, um mechanischen Verschleiß zu minimieren.
- **Positionieraufgaben**: Rampenbegrenzung für Servoachsen oder Schrittmotoren, bei denen ein gleichmäßiges Anfahren an eine Zielposition erforderlich ist.
- **Signalglättung**: Glättung von Prozesswerten in der Messtechnik, um Störspitzen abzufangen und einen stabilen Regelkreis zu gewährleisten.
- **Test- und Prüfstände**: Erzeugung definierter Anfahr- und Abschaltkurven für Prüfzyklen.

## Vergleich mit ähnlichen Bausteinen

Gegenüber dem ursprünglichen `RampLimitFS` bietet **AUDI_RampLimitFS** eine vereinheitlichte Schnittstelle für den direkten Anschluss an AUDI-basierte Signalpfade und verhindert so manuelle Konvertierungs- und Verdrahtungsaufwände. Zusätzlich werden die Grenzwertstatusinformationen gelatcht, sodass die Signalauswertung unabhängig von der Dauer des Grenzzustands erfolgen kann. Im Vergleich zu einem einfachen PID- oder Integrator-Baustein ist hier nur eine begrenzte Anzahl von Ereignissen und Einstellparametern vorhanden, was den Baustein insbesondere für einfache, robuste Rampenfunktionen in Echtzeitanwendungen prädestiniert.

## Fazit

**AUDI_RampLimitFS** ist ein modular aufgebauter Rampenbaustein, der die Kernfunktionalität eines klassischen Rampengenerators mit modernen Adapterkonzepten und Statuslatch-Funktionen kombiniert. Die klare Schnittstelle, die automatische Datentypkonvertierung und die zwei Rampengeschwindigkeiten machen ihn zu einer vielseitig einsetzbaren Komponente in Automatisierungssystemen. Durch die Kapselung komplexer internen Logik wird der Anwender von typischen Implementierungsdetails befreit und kann den Baustein direkt in seine Signalverarbeitung integrieren.

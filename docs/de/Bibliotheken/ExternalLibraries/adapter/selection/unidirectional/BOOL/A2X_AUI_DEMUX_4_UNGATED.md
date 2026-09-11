# A2X_AUI_DEMUX_4_UNGATED

![A2X_AUI_DEMUX_4_UNGATED](./A2X_AUI_DEMUX_4_UNGATED.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein `A2X_AUI_DEMUX_4_UNGATED` ist ein adapterbasierter 1-aus-4-Demultiplexer. Er empfängt über einen `A2X`-Adapter einen Eingangswert und leitet diesen über einen von vier `A2X`-Ausgangsadaptern weiter. Die Auswahl des aktiven Ausgangs erfolgt über einen `AUI`-Adapter, der den Index `K` transportiert.

Die Besonderheit dieser Variante ist der Zusatz `UNGATED`: Im Gegensatz zu einem Baustein mit Änderungserkennung wird hier **jedes** neu berechnete Ergebnis bedingungslos weitergegeben – auch wenn sich der Wert gegenüber dem vorherigen Zyklus nicht geändert hat. Dadurch eignet sich der Baustein besonders für Verbraucher, die eine periodische Kadenz benötigen, zum Beispiel für Ableitungs- oder Frequenzberechnungen.

## Schnittstellenstruktur

Der Baustein besitzt außer dem Ereignisausgang `CNF` keine direkten Ereignis- oder Datenports. Alle Eingangs- und Ausgangswerte werden über Adapter übertragen.

### **Ereignis-Eingänge**

Keine eigenständigen Ereignis-Eingänge definiert.

Die Steuerung und Datenübertragung erfolgt über die Adapter `K` und `IN`.

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `CNF` | Event | Bestätigung des Set-Index-K-Vorgangs |

### **Daten-Eingänge**

Keine direkten Daten-Eingänge.

Die Eingangsdaten sind in den Adapter-Sockets `K` und `IN` gekapselt.

### **Daten-Ausgänge**

Keine direkten Daten-Ausgänge.

Die Ausgangsdaten werden über die Adapter-Plugs `OUT1` bis `OUT4` bereitgestellt.

### **Adapter**

| Name | Richtung / Rolle | Adaptertyp | Beschreibung |
|---|---|---|---|
| `K` | Socket / Eingang | `adapter::types::unidirectional::AUI` | Index zur Auswahl des aktiven Ausgangs |
| `IN` | Socket / Eingang | `adapter::types::unidirectional::A2X` | Eingangswert, der demultiplext wird |
| `OUT1` | Plug / Ausgang | `adapter::types::unidirectional::A2X` | Ausgangswert 1, ausgewählt bei `K = 0` |
| `OUT2` | Plug / Ausgang | `adapter::types::unidirectional::A2X` | Ausgangswert 2, ausgewählt bei `K = 1` |
| `OUT3` | Plug / Ausgang | `adapter::types::unidirectional::A2X` | Ausgangswert 3, ausgewählt bei `K = 2` |
| `OUT4` | Plug / Ausgang | `adapter::types::unidirectional::A2X` | Ausgangswert 4, ausgewählt bei `K = 3` |

## Funktionsweise

Der Baustein arbeitet als Adapter-basierter Demultiplexer:

1. Über den Adapter `K` wird der Auswahlindex empfangen.
2. Über den Adapter `IN` wird der aktuelle Eingangswert empfangen.
3. Der Baustein leitet diesen Wert unverändert an den Ausgang weiter, der dem Index `K` entspricht.
4. Nach der Übernahme des Index wird das Ereignis `CNF` ausgelöst.
5. Da die Variante `UNGATED` keine Änderungserkennung besitzt, wird jeder ankommende Ergebniszyklus weitergegeben – auch bei unveränderten Werten.

Nicht ausgewählte Ausgänge werden in der Regel nicht bedient und behalten ihren bisherigen Zustand.

## Technische Besonderheiten

- **Keine Änderungserkennung:** Jedes neu berechnete Ergebnis wird ungefiltert weitergeleitet.
- **Reine Adapter-Anbindung:** Es gibt keine direkten Daten- oder Ereignis-Eingänge; der Datenaustausch erfolgt über die unidirektionalen Adapter `A2X` und `AUI`.
- **Generischer Funktionsbaustein:** Der Baustein ist als generischer FB mit `GenericClassName = 'GEN_A2X_AUI_DEMUX'` deklariert.
- **Erweiterungskonzept:** Die Bezeichnung `A2X` verweist auf die Verwendung des `A2X`-Adaptertyps für die eigentlichen Nutzdaten.
- **Backend-Hinweis:** Die Versionsinformation weist darauf hin, dass das Adapter-Backend `GEN_A2X_AUI_DEMUX` noch zu implementieren ist. Vor dem Einsatz sollte geprüft werden, ob das verwendete Laufzeitsystem diesen generischen Baustein bereits unterstützt.
- **Lizenz:** Der Baustein ist unter der Eclipse Public License 2.0 verfügbar.

## Zustandsübersicht

Der Baustein besitzt keinen explizit sichtbaren Zustandsautomaten. Der relevante Zustand ist der aktuell eingestellte Index `K`.

| Index `K` | aktiver Ausgang |
|---|---|
| `0` | `OUT1` |
| `1` | `OUT2` |
| `2` | `OUT3` |
| `3` | `OUT4` |

Für Indexwerte außerhalb von `0..3` ist in der Typdefinition kein Verhalten festgelegt.

## Anwendungsszenarien

- **Ableitungs- und Frequenzberechnungen:** Verbraucher, die jeden Takt benötigen, auch wenn sich der Messwert nicht ändert.
- **Parallele Auswertung:** Ein analoger oder generischer Datenwert wird auf vier unterschiedliche Auswertungsmodule verteilt.
- **Datenstrom-Umschaltung:** Ein ankommender Wert wird abhängig vom Index `K` an genau eine von vier Senken weitergereicht.
- **Periodische Diagnose:** Rohdaten werden ohne Filterung und ohne Verlust von Wiederholungen an Beobachter oder Analysefunktionen übergeben.
- **Adapterbasierte Systeme:** Der Baustein eignet sich für modulare Architekturen, die mit unidirektionalen `A2X`- und `AUI`-Adaptern arbeiten.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Eigenschaft |
|---|---|
| `A2X_AUI_DEMUX_4` | Leitet Werte nur bei Änderung des Eingangswerts weiter. |
| `A2X_AUI_DEMUX_4_UNGATED` | Leitet jeden Ergebniszyklus bedingungslos weiter, unabhängig von einer Wertänderung. |
| `AX_AUI_DEMUX_4_UNGATED` | Vergleichbare Demultiplexer-Variante mit anderem Adapter-Backend; der vorliegende Baustein ist die `A2X`-basierte Variante. |

Der entscheidende Unterschied zu nicht-`UNGATED`-Varianten ist das bewusste Fehlen der Änderungserkennung. Dadurch ist die `UNGATED`-Variante besser geeignet, wenn eine gleichbleibende, periodische Weitergabe von Werten wichtiger ist als die Unterdrückung redundanter Daten.

## Fazit

`A2X_AUI_DEMUX_4_UNGATED` ist ein kompakter, adapterbasierter 1-aus-4-Demultiplexer ohne Wertänderungsfilterung. Er eignet sich besonders für Anwendungen, die eine kontinuierliche und periodische Datenweitergabe benötigen, wie etwa Ableitungs- oder Frequenzberechnungen.

Durch die rein adapterbasierte Schnittstelle lässt sich der Baustein gut in modulare 4diac-Projekte integrieren. Vor dem Einsatz muss sichergestellt werden, dass das zugehörige generische Backend `GEN_A2X_AUI_DEMUX` in der verwendeten Laufzeitumgebung verfügbar ist.

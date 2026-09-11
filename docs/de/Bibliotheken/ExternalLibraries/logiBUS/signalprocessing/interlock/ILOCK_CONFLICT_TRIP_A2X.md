# ILOCK_CONFLICT_TRIP_A2X


![ILOCK_CONFLICT_TRIP_A2X_ecc](./ILOCK_CONFLICT_TRIP_A2X_ecc.svg)

![ILOCK_CONFLICT_TRIP_A2X](./ILOCK_CONFLICT_TRIP_A2X.svg)

* * * * * * * * * *
## Einleitung

Der Funktionsblock **ILOCK_CONFLICT_TRIP_A2X** realisiert eine sicherheitsrelevante Interlock-Logik für bidirektionale Antriebe oder Bewegungsachsen. Er nimmt über einen A2X-Adapter (unidirektional) die Signale **UP** und **DOWN** entgegen und gibt diese über einen weiteren A2X-Adapter an die nachgelagerte Steuerung weiter. Das Besondere ist die Konfliktüberwachung: Werden beide Eingänge gleichzeitig aktiv, wird ein **Trip-Zustand** ausgelöst, der als Alarm über einen separaten AX-Adapter ausgegeben wird. Erst nach einem expliziten **Reset** und dem Deaktivieren beider Eingänge kehrt der Baustein in den Normalbetrieb zurück.

Der Baustein eignet sich für Anwendungen, bei denen eine wechselseitige Verriegelung (z. B. Auf/Ab oder Vor/Zurück) erforderlich ist und ein gleichzeitiges Ansteuern beider Richtungen als gefährlicher Fehlerzustand behandelt werden muss.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
| Name | Typ | Beschreibung |
|------|-----|--------------|
| `EI_RESET` | Event | Setzt den Baustein nach einem Trip zurück, sofern keine der Eingangsgrößen `IN.UP` oder `IN.DOWN` mehr aktiv sind. |

**Ereignis-Ausgänge**  
Es sind keine direkten Ereignis-Ausgänge vorhanden. Die Ausgabe erfolgt ausschließlich über die Adapter `OUT` und `TRIP_OUT`, die jeweils Ereignissignale wie `OUT.E_UP`, `OUT.E_DOWN` und `TRIP_OUT.E1` auslösen.

### **Daten-Eingänge**
Direkte Dateneingänge existieren nicht. Alle Eingabedaten werden über den **Adapter `IN`** empfangen:
- `IN.UP` (BOOL): Signal „Aufwärts/Vorwärts“  
- `IN.DOWN` (BOOL): Signal „Abwärts/Rückwärts“

### **Daten-Ausgänge**
Die Ausgabedaten werden über die Adapter `OUT` und `TRIP_OUT` bereitgestellt:
- `OUT.UP` (BOOL): Weiterleitung des Zustands „Aufwärts/Vorwärts“  
- `OUT.DOWN` (BOOL): Weiterleitung des Zustands „Abwärts/Rückwärts“  
- `TRIP_OUT.D1` (BOOL): Signalisiert den Trip-Zustand (TRUE = Fehler/Konflikt erkannt)

### **Adapter**
| Name | Typ | Richtung | Beschreibung |
|------|-----|----------|--------------|
| `IN` | `adapter::types::unidirectional::A2X` | Socket | Empfängt die Eingangssignale `UP` und `DOWN` sowie die zugehörigen Ereignisse `E_UP` und `E_DOWN`. |
| `OUT` | `adapter::types::unidirectional::A2X` | Plug | Gibt die verarbeiteten Ausgangssignale `UP` und `DOWN` sowie die Ereignisse `E_UP` und `E_DOWN` an die angeschlossene Logik weiter. |
| `TRIP_OUT` | `adapter::types::unidirectional::AX` | Plug | Gibt das Trip-Signal `D1` und das Ereignis `E1` bei einem erkannten Konflikt aus. |

## Funktionsweise

Der Funktionsblock arbeitet als **zustandsbasierter Verriegelungsbaustein**. Er besitzt vier Zustände:

- **STOP**: Beide Richtungssignale sind inaktiv. Es werden `OUT.UP = FALSE`, `OUT.DOWN = FALSE` und `TRIP_OUT.D1 = FALSE` ausgegeben.
- **UP**: Nur `IN.UP` ist aktiv. Der Baustein setzt `OUT.UP = TRUE` und `OUT.DOWN = FALSE`. Der Trip wird zurückgesetzt.
- **DOWN**: Nur `IN.DOWN` ist aktiv. Der Baustein setzt `OUT.DOWN = TRUE` und `OUT.UP = FALSE`. Der Trip wird zurückgesetzt.
- **TRIP**: Beide Eingänge sind gleichzeitig aktiv oder es wird während eines aktiven Zustands der jeweils andere Eingang aktiviert. In diesem Fall wird `TRIP_OUT.D1 = TRUE` gesetzt und beide Ausgänge `OUT.UP` und `OUT.DOWN` werden deaktiviert.

Die Zustandsübergänge werden durch die Ereignisse `IN.E_UP` und `IN.E_DOWN` ausgelöst, wobei die zugehörigen Bedingungen die Werte von `IN.UP` und `IN.DOWN` prüfen. Ein Trip kann aus jedem aktiven Zustand (STOP, UP oder DOWN) heraus ausgelöst werden, sobald beide Eingangssignale gleichzeitig TRUE sind. Der Baustein bleibt solange im TRIP-Zustand, bis ein `EI_RESET`-Ereignis eintritt und beide Eingänge wieder FALSE sind.

## Technische Besonderheiten

- **Konfliktpriorisierung**: Der Baustein priorisiert nicht – er erkennt jeden Konflikt und geht in den sicheren Zustand „TRIP“. Ein gleichzeitiges Ansteuern beider Richtungen wird als Fehler behandelt.
- **Reset-Bedingung**: Ein Zurücksetzen aus dem Trip ist nur möglich, wenn keine der beiden Eingangsgrößen (`IN.UP`, `IN.DOWN`) mehr aktiv ist. Dies verhindert ein unbeabsichtigtes Quittieren während eines anhaltenden Fehlers.
- **Ereignisbasierte Verarbeitung**: Die Zustandswechsel erfolgen ausschließlich über die Ereignisse `IN.E_UP` und `IN.E_DOWN`. Die Ausgangsereignisse (`OUT.E_UP`, `OUT.E_DOWN`, `TRIP_OUT.E1`) werden bei jedem Zustandswechsel erzeugt, auch wenn sich die Datenwerte nicht ändern (z. B. beim Übergang von STOP auf TRIP).
- **Adapter-Kapselung**: Die Verwendung von standardisierten A2X/AX-Adaptern ermöglicht eine einfache Integration in bestehende 4diac- oder IEC-61499-Systeme, ohne dass zusätzliche Datentypen definiert werden müssen.
- **Typischer Einsatz**: Geeignet für sicherheitsgerichtete Steuerungen von Industrieantrieben, Verriegelungen von Schiebetüren, Hebezeugen oder anderen Positionieranlagen.

## Zustandsübersicht

| Zustand | Bedingung für Eintritt | Ausgangssignale | Zustand nach Ereignis |
|---------|------------------------|-----------------|------------------------|
| **STOP** | Initial oder nach Reset, wenn keine Eingänge aktiv. | `OUT.UP = F`, `OUT.DOWN = F`, `TRIP_OUT.D1 = F` | Bei `IN.E_UP` mit `IN.UP` und nicht `IN.DOWN` → UP; bei `IN.E_DOWN` mit `IN.DOWN` und nicht `IN.UP` → DOWN; bei beiden aktiv → TRIP. |
| **UP** | Aus STOP, wenn nur `IN.UP` aktiv und `IN.E_UP` eintritt. | `OUT.UP = T`, `OUT.DOWN = F`, `TRIP_OUT.D1 = F` | Bei `IN.E_UP` mit `NOT IN.UP` → STOP; bei `IN.E_DOWN` mit `IN.DOWN` → TRIP. |
| **DOWN** | Aus STOP, wenn nur `IN.DOWN` aktiv und `IN.E_DOWN` eintritt. | `OUT.UP = F`, `OUT.DOWN = T`, `TRIP_OUT.D1 = F` | Bei `IN.E_DOWN` mit `NOT IN.DOWN` → STOP; bei `IN.E_UP` mit `IN.UP` → TRIP. |
| **TRIP** | Aus jedem Zustand, sobald beide Eingänge aktiv sind (Konflikt). | `OUT.UP = F`, `OUT.DOWN = F`, `TRIP_OUT.D1 = T` | Bei `EI_RESET` mit `NOT IN.UP AND NOT IN.DOWN` → STOP. |

*Hinweis: F = FALSE, T = TRUE*

## Anwendungsszenarien

- **Antriebssteuerung mit Verriegelung**: Steuerung eines Gleichstrommotors, der nur entweder vorwärts oder rückwärts laufen darf. Die beiden Richtungssignale kommen von externen Sensoren oder einer SPS. Ein gleichzeitiges Aktivieren beider Signale (z. B. durch defekte Sensorik) führt zu einem sofortigen Stopp und einer Alarmanzeige.
- **Automatische Schiebetüren**: Binäre Signale für „Auf“ und „Zu“ werden überwacht. Ein gleichzeitiges Auftreten wird als Fehler erkannt und der Trip-Ausgang aktiviert, was ein Notstopp-Szenario einleiten kann.
- **Sicherheits-Logik für Hebezeuge**: Verriegelung von Hub- und Senkbewegungen. Bei Konflikt wird der Antrieb abgeschaltet und eine Wartungsmeldung ausgegeben.
- **Integration in 4diac-Systeme**: Dank der standardisierten Adapter kann der Baustein in verteilte Steuerungssysteme eingebunden werden, ohne dass die Signaltypen angepasst werden müssen.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu einfachen Verriegelungsbausteinen, die z. B. nur die Priorität festlegen (z. B. „UP hat Vorrang“), erkennt **ILOCK_CONFLICT_TRIP_A2X** jegliche Konflikte und löst einen festen Trip aus. Dies ist für sicherheitsgerichtete Anwendungen wichtiger als eine Priorisierung, da ein gleichzeitiges Aktivsein beider Richtungen immer ein potenziell gefährliches Ereignis darstellt. Andere Bausteine könnten eine mehrstufige Verriegelung mit zeitlichen Verzögerungen implementieren; dieser Baustein verzichtet bewusst darauf, um eine schnelle und deterministische Reaktion zu gewährleisten. Der Unterschied zu ähnlichen A2X-basierten Bausteinen liegt vor allem in der expliziten Konflikterkennung und der Notwendigkeit eines manuellen Resets.

## Fazit

Der **ILOCK_CONFLICT_TRIP_A2X** ist ein robustes und einfach zu integrierendes Verriegelungsmodul für Anwendungen, bei denen zwei sich gegenseitig ausschließende Signale überwacht werden müssen. Seine ereignisbasierte Implementierung ermöglicht eine schnelle Reaktionszeit, während die klare Zustandsmaschine und die standardisierten Adapter eine unkomplizierte Einbindung in IEC-61499-Systeme gewährleisten. Die bewusste Entscheidung, Konflikte immer als Trip zu behandeln, macht ihn zu einer zuverlässigen Komponente für sicherheitsbewusste Steuerungslösungen.
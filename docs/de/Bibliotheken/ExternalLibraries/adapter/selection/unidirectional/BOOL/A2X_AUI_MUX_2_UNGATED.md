# A2X_AUI_MUX_2_UNGATED

![A2X_AUI_MUX_2_UNGATED](./A2X_AUI_MUX_2_UNGATED.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein `A2X_AUI_MUX_2_UNGATED` ist ein generischer Multiplexer, der zwei A2X-Eingänge über einen AUI-Index auswählt und das gewählte Signal über einen A2X-Ausgang bereitstellt. Im Gegensatz zur Variante `A2X_AUI_MUX_2` besitzt dieser Baustein keine Änderungserkennung: Jedes neu berechnete Ergebnis wird unabhängig davon, ob sich der Wert geändert hat, bedingungslos weitergegeben. Dadurch eignet er sich besonders für Anwendungen, die eine periodische Kadenz benötigen, etwa Ableitungs- oder Frequenzberechnungen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Baustein besitzt keine Ereignis-Eingänge. Die Steuerung erfolgt ausschließlich über die Adapter-Schnittstellen.

### **Ereignis-Ausgänge**

| Ereignis | Kommentar |
|----------|-----------|
| `CNF` | Bestätigung der Übernahme des Auswahl-Index K (Confirmation of Set Index K). |

### **Daten-Eingänge**

Der Baustein besitzt keine direkten Daten-Eingänge. Alle Daten werden über die Adapter-Schnittstellen `IN1` und `IN2` sowie den Auswahl-Adapter `K` übergeben.

### **Daten-Ausgänge**

Der Baustein besitzt keine direkten Daten-Ausgänge. Der ausgewählte Wert wird über den Adapter-Ausgang `OUT` bereitgestellt.

### **Adapter**

| Adapter | Richtung | Typ | Kommentar |
|---------|----------|-----|-----------|
| `K`   | Socket (Eingang) | `adapter::types::unidirectional::AUI` | Auswahl-Index: Bei K = 0 wird IN1 gewählt, bei K = 1 wird IN2 gewählt. |
| `IN1` | Socket (Eingang) | `adapter::types::unidirectional::A2X` | Erster Eingangswert (ausgewählt bei K = 0). |
| `IN2` | Socket (Eingang) | `adapter::types::unidirectional::A2X` | Zweiter Eingangswert (ausgewählt bei K = 1). |
| `OUT` | Plug (Ausgang) | `adapter::types::unidirectional::A2X` | Ausgang mit dem ausgewählten Eingangswert. |

## Funktionsweise

Der Baustein empfängt über den Adapter `K` einen Auswahl-Index. Abhängig von diesem Index wird der Wert von `IN1` (bei K = 0) oder von `IN2` (bei K = 1) an den Ausgang `OUT` durchgereicht. Nach erfolgreicher Übernahme des Index wird das Ereignis `CNF` ausgegeben, um die Verarbeitung zu bestätigen.

Die Kennzeichnung `UNGATED` bedeutet, dass keine Änderungserkennung zwischen altem und neuem Wert stattfindet. Jeder am ausgewählten Eingang anliegende neue Datenwert wird sofort und vollständig an den Ausgang weitergegeben. Es findet keine Filterung identischer Werte statt. Dadurch wird sichergestellt, dass nachfolgende Verbraucher, die auf eine gleichmäßige zeitliche Abfolge angewiesen sind, kontinuierlich mit Daten versorgt werden.

## Technische Besonderheiten

- **Generischer Baustein**: Der Baustein ist als generischer FB mit dem Klassennamen `GEN_A2X_AUI_MUX` definiert. Der tatsächliche Typ wird erst bei der Instanziierung im Projekt aufgelöst.
- **Reine Adapter-Schnittstelle**: Es existieren keine klassischen Ereignis- oder Daten-Pins. Die gesamte Kommunikation läuft über die Adapter `K`, `IN1`, `IN2` und `OUT`.
- **Unidirektionale Adapter**: Alle verwendeten Adapter sind vom Typ `unidirectional`, d.h. die Datenflussrichtung ist fest vorgegeben und es findet keine Rückkanal-Kommunikation statt.
- **Keine Änderungserkennung**: Identische Datenwerte werden dennoch als Ereignis weitergegeben. Dies unterscheidet den Baustein von der Variante mit Änderungserkennung.
- **Bestätigungsmechanismus**: Das Ereignis `CNF` signalisiert, dass der Auswahl-Index K übernommen und die Umschaltung abgeschlossen wurde.

## Zustandsübersicht

Der Baustein lässt sich konzeptionell in drei Zustände gliedern:

1. **Bereit**: Der Baustein wartet auf einen neuen Auswahl-Index über den Adapter `K`.
2. **Auswahl**: Der an `K` anliegende Index wird ausgewertet und der entsprechende Eingang (`IN1` oder `IN2`) ausgewählt.
3. **Weiterleitung**: Der Wert des ausgewählten Eingangs wird unmittelbar an den Ausgang `OUT` durchgereicht und das Ereignis `CNF` ausgelöst.

Eine interne Speicherung vorheriger Werte oder ein Vergleich mit dem aktuellen Wert findet nicht statt.

## Anwendungsszenarien

- **Periodische Datenweitergabe**: Wenn ein nachgeschalteter Baustein eine feste Datenrate benötigt, unabhängig davon, ob sich der Messwert ändert.
- **Ableitungs- und Frequenzberechnungen**: Hier werden zeitliche Änderungen benötigt; ein Auslassen identischer Werte würde die Berechnung verfälschen.
- **Umschaltung zwischen zwei Signalquellen**: Der Baustein kann verwendet werden, um zwischen zwei Sensoren oder Datenströmen umzuschalten, wobei jede neue Messung am ausgewählten Eingang sofort weitergeleitet wird.
- **Test- und Diagnosezwecke**: Wenn sichergestellt werden soll, dass jede Berechnung am Ausgang ankommt, auch wenn sich der Wert nicht ändert.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Änderungserkennung | Ausgabe identischer Werte |
|----------|--------------------|---------------------------|
| `A2X_AUI_MUX_2` | Ja, nur bei Wertänderung wird ausgegeben. | Nein, identische Werte werden unterdrückt. |
| `A2X_AUI_MUX_2_UNGATED` | Nein, jede neue Berechnung wird ausgegeben. | Ja, auch bei unverändertem Wert wird ein Ereignis erzeugt. |

Der wesentliche Unterschied liegt also in der Behandlung unveränderter Werte. Während der normale Multiplexer die Ausgabe bei unverändertem Wert unterdrückt, gibt die `UNGATED`-Variante das Ergebnis immer aus.

## Fazit

Der Funktionsbaustein `A2X_AUI_MUX_2_UNGATED` ist eine spezialisierte Variante eines 2-zu-1-Multiplexers mit Adapter-basierter Kommunikation. Durch den Verzicht auf eine Änderungserkennung wird eine lückenlose und kadenzgenaue Datenweitergabe ermöglicht. Er ist damit ideal für Anwendungen, die auf eine kontinuierliche Verarbeitung angewiesen sind und keine Werte auslassen dürfen.

# T_FF_EVENT_AX


![T_FF_EVENT_AX_network](./T_FF_EVENT_AX_network.svg)

![T_FF_EVENT_AX](./T_FF_EVENT_AX.svg)

* * * * * * * * * *
## Einleitung
Der Baustein **T_FF_EVENT_AX** ist eine Subapp, die ein einfaches Toggle-Flip-Flop realisiert. Jeder Ereignisimpuls am Eingang `IND` kippt den Zustand des Adapter-Ausgangs `Q`. Die Implementierung verwendet die modularen 4diac-Bausteine `AX_E_SWITCH`, `AX_SR` und `AX_SPLIT_2`. Sie ist generisch aufgebaut und besitzt keine Hardware-Abhängigkeit, wodurch sie in verschiedenen Umgebungen eingesetzt werden kann.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
- **IND** : Ereignis (Event) – Jeder Impuls an diesem Eingang löst einen Schaltvorgang aus.

### **Ereignis-Ausgänge**
- Keine.

### **Daten-Eingänge**
- Keine.

### **Daten-Ausgänge**
- Keine.

### **Adapter**
- **Q** : Ausgang (Typ `adapter::types::unidirectional::AX`) – Liefert den aktuellen Zustand des Flip-Flops (0 oder 1) als universelles Adaptersignal.

## Funktionsweise
Der Baustein arbeitet als Toggle-Flip-Flop auf Ereignisbasis. Ein eingehendes Ereignis an `IND` wird an den internen Baustein `AX_E_SWITCH` weitergeleitet. Dieser vergleicht den aktuellen Zustand von `Q` (zurückgeführt über `AX_SPLIT_2.OUT2` an den Eingang `G`) und löst daraufhin entweder den Ausgang `EO0` oder `EO1` aus:

- Ist `Q` = 0 (Low), wird `EO0` aktiviert und an den Set-Eingang `S` des `AX_SR`-Flip-Flops gesendet.
- Ist `Q` = 1 (High), wird `EO1` aktiviert und an den Reset-Eingang `R` des `AX_SR`-Flip-Flops gesendet.

Der `AX_SR` (Set/Reset-Flip-Flop) reagiert auf die Ereignisse und setzt bzw. löscht seinen internen Zustand. Der Ausgang `Q` von `AX_SR` wird über den Splitter `AX_SPLIT_2` auf zwei Pfade verteilt: `OUT1` führt zum externen Ausgang `Q` der Subapp, `OUT2` wird als Rückkopplung an `AX_E_SWITCH` übergeben. Dadurch wird bei jedem neuen `IND`-Ereignis der Zustand deterministisch umgeschaltet.

Die Schaltung realisiert damit ein klassisches Toggle-Verhalten: Jeder Klick oder Impuls invertiert den logischen Pegel am Ausgang.

## Technische Besonderheiten
- **Modularer Aufbau** : Die Funktionalität wird durch Kombination einfacher, wiederverwendbarer Bausteine erreicht.
- **Rückkopplung** : Über den Splitter wird der aktuelle Zustand intern zurückgeführt, um die Schaltrichtung korrekt zu bestimmen (Set vs. Reset).
- **Keine Hardwareabhängigkeit** : Die Subapp arbeitet rein ereignisbasiert und kann ohne Änderungen in verschiedenen Steuerungsumgebungen eingesetzt werden.
- **Einfache Schnittstelle** : Nur ein Ereigniseingang und ein Adapterausgang, daher sehr leicht integrierbar.

## Zustandsübersicht
| Zustand | Bedeutung | Reaktion auf `IND` |
|---------|-----------|--------------------|
| `Q = 0` | Ausgang Low | Set-Ereignis an `AX_SR` → Zustand wechselt zu `Q = 1` |
| `Q = 1` | Ausgang High | Reset-Ereignis an `AX_SR` → Zustand wechselt zu `Q = 0` |

Das Flip-Flop besitzt zwei stabile Zustände, die sich bei jedem Ereignis am Eingang abwechseln.

## Anwendungsszenarien
- **Taster-Umschaltung** : Ein einfacher Taster oder Schalter kann verwendet werden, um zwischen zwei Betriebszuständen (z. B. Ein/Aus, Hoch/Runter) umzuschalten.
- **Ereigniszähler** : Durch Zählen der Impulse kann der Ausgang als Halbzähler (Toggle) verwendet werden.
- **Zustandssteuerung** : Steuerung von binären Ausgängen in Automatisierungsprozessen, bei denen sich der Zustand bei jedem Impuls ändern soll.

## Vergleich mit ähnlichen Bausteinen
Es gibt in 4diac fertige Bausteine wie `SR` oder `RS`-Flip-Flops, die ebenfalls Set/Reset-Funktion bieten. Direkte Toggle-Bausteine sind jedoch oft nicht als Einzel-FB vorhanden. Der hier vorgestellte `T_FF_EVENT_AX` kombiniert vorhandene Elemente zu einem Toggle-Verhalten, ohne dass eine eigene zyklische Zustandslogik geschrieben werden muss. Gegenüber einem spezialisierten Toggle-FB bietet er den Vorteil, dass er vollständig aus standardisierten, wiederverwendbaren Komponenten aufgebaut ist und dadurch leichter angepasst oder erweitert werden kann.

## Fazit
`T_FF_EVENT_AX` ist eine kompakte, modulare Lösung zur Realisierung eines Toggle-Flip-Flops in IEC 61499-Umgebungen. Der Aufbau aus bewährten Bausteinen gewährleistet Robustheit und Wiederverwendbarkeit. Durch die klare Schnittstelle (nur ein Ereigniseingang und ein Adapterausgang) ist die Integration in übergeordnete Steuerungssysteme unkompliziert. Besonders geeignet ist er für Anwendungen, in denen jeder Impuls den Zustand eines binären Ausgangs wechseln soll.
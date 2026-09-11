# TimeTicker

![TimeTicker](./TimeTicker.svg)

* * * * * * * * * *
## Einleitung

Der Funktionsblock **TimeTicker** ist ein zusammengesetzter Baustein (Composite FB), der einen zyklischen Zeitgeber realisiert. Er basiert auf dem Standardbaustein `E_CYCLE` und erweitert dessen Funktionalität durch eine Adapter-Schnittstelle zur Kommunikation mit anderen Bausteinen. Der TimeTicker erzeugt periodische Ereignisse (Ticks) und stellt dabei Informationen über die aktuelle Zykluszeit, die vergangene Zeit und den Prozesszustand bereit. Er eignet sich für Anwendungen, in denen eine zeitgesteuerte Ablaufsteuerung mit Rückmeldung über den aktuellen Zustand benötigt wird, beispielsweise in der Prozessautomatisierung oder bei zeitbasierten Steuerungsaufgaben.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ | Kommentar |
|----------|-----|-----------|
| `INIT` | EInit | Initialisierungsanforderung. Mit `TC` verbunden. |

### **Ereignis-Ausgänge**

| Ereignis | Typ | Kommentar |
|----------|-----|-----------|
| `INITO` | EInit | Bestätigung der Initialisierung. |
| `CNF` | Event | Meldet bei jedem Tick den aktuellen Wert von `Q` und `ET`. |
| `STARTO` | Event | Ausgangssignal zum Starten des Tickers (wird vom Adapter weitergegeben). |
| `STOPO` | Event | Ausgangssignal zum Stoppen des Tickers (wird vom Adapter weitergegeben). |

### **Daten-Eingänge**

| Name | Typ | Initialwert | Kommentar |
|------|-----|-------------|-----------|
| `TC` | TIME | T#200ms | Zykluszeit (cycle time). |

### **Daten-Ausgänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `Q` | BOOL | Ausgangszustand (z. B. aktiv/inaktiv). |
| `PT` | TIME | Prozesszeit (process time). |
| `ET` | TIME | Verstrichene Zeit (elapsed time). |

### **Adapter**

| Typ | Name | Richtung |
|-----|------|----------|
| `adapter::events::TimeOut::ATimeTick` | `TimeTickSocket` | Plug (Stecker) |

Der Adapter `TimeTickSocket` dient als Schnittstelle zu einem übergeordneten Zeitgeber-Kommunikationsprotokoll. Er empfängt Start-/Stopp-Ereignisse vom Adapter und sendet Tick-Bestätigungen sowie Zeitdaten an den angeschlossenen Adapter.

## Funktionsweise

Der TimeTicker nutzt intern den Baustein `E_CYCLE`, der einen periodischen Zyklus erzeugt. Die Funktion im Detail:

1. **Initialisierung**: Beim Ereignis `INIT` wird der Baustein initialisiert und das Ereignis `INITO` sofort ausgelöst, um die Bereitschaft zu bestätigen.
2. **Start/Stopp über Adapter**: Die Ereignisse `STARTO_IN` und `STOPO_IN` vom Adapter (über `TimeTickSocket`) werden direkt an den internen `E_CYCLE` weitergeleitet, um den Zyklus zu starten bzw. zu stoppen. Gleichzeitig werden diese Ereignisse als `STARTO` bzw. `STOPO` an den Ausgang gegeben, damit angeschlossene Bausteine den Zustandswechsel ebenfalls mitbekommen.
3. **Tick-Erzeugung**: Der `E_CYCLE` erzeugt bei jedem Ablauf des Zyklus (konfiguriert über `TC`) das Ereignis `EO`. Dieses wird über den Adapter (`TimeTickSocket.REQ`) nach außen gesendet (z. B. an einen anderen Baustein).
4. **Rückmeldung**: Nach jedem Tick kommt vom Adapter das Ereignis `CNF` (Confirmation), das die aktuellen Werte von `Q`, `ET` und `PT` mit sich führt. Dieses Ereignis wird als `CNF` am Ausgang des TimeTickers nach außen gegeben. Die Daten `Q`, `ET` und `PT` werden aus den vom Adapter gelieferten Werten übernommen und an den jeweiligen Datenausgängen bereitgestellt.
5. **Datenfluss**: Die Zykluszeit `TC` wird direkt als `DT` (Delta-Time) an den `E_CYCLE` übergeben. Die Ausgabedaten `Q`, `PT` und `ET` stammen aus dem Adapter (`TimeTickSocket.Q`, `TimeTickSocket.PT`, `TimeTickSocket.ET`) und werden an die entsprechenden Ausgangs-Pins weitergeleitet.

Somit fungiert der TimeTicker als eine Art Wrapper, der die Steuerung eines zyklischen Zeitgebers über einen Adapter ermöglicht und die dabei anfallenden Zeitdaten nach außen transparent macht.

## Technische Besonderheiten

- **Adapterbasiert**: Die Kommunikation mit externen Bausteinen erfolgt ausschließlich über den Adapter `TimeTickSocket`. Dadurch ist der TimeTicker klar vom eigentlichen Zeitgeber-Mechanismus entkoppelt.
- **Durchreichung von Ereignissen**: Start- und Stopp-Signale werden sowohl an den internen Zyklus als auch direkt an die Ausgänge weitergegeben – dies erlaubt eine Synchronisation mehrerer Bausteine.
- **Initialisierung ohne Verzögerung**: Das `INIT`-Ereignis wird sofort mit `INITO` beantwortet, ohne dass ein interner Zustandswechsel abgewartet wird.
- **Nutzung von `E_CYCLE`**: Die eigentliche Zeitsteuerung basiert auf dem Standard-FB `E_CYCLE` aus der IEC 61499 Bibliothek, was eine bewährte und robuste Implementierung garantiert.
- **Zeitwerte**: Die Ausgabevariablen sind vom Typ `TIME` und erlauben eine präzise Verarbeitung im Mikrosekundenbereich.

## Zustandsübersicht

Der TimeTicker besitzt keinen expliziten internen Zustandsautomaten, da er als Composite FB hauptsächlich die Zustände des internen `E_CYCLE` widerspiegelt. Man kann jedoch folgende logische Zustände ableiten:

- **Initialisiert**: Nach `INIT`/`INITO` ist der Baustein bereit.
- **Gestoppt**: Solange kein Start-Signal empfangen wurde oder nach einem Stopp-Signal, erzeugt der Ticker keine Ticks.
- **Laufend**: Nach Erhalt eines Start-Signals (über Adapter) läuft der Zyklus und erzeugt periodisch `EO`/`CNF`-Ereignisse.

Die Übergänge zwischen diesen Zuständen werden durch die Ereignisse `STARTO_IN` (Start) und `STOPO_IN` (Stopp) gesteuert, die über den Adapter eingehen.

## Anwendungsszenarien

- **Zeitbasierte Ablaufsteuerung**: Einsatz in Fertigungsprozessen, wo periodische Aktionen ausgelöst werden müssen (z. B. Sensorabfragen, Datenlogging).
- **Kommunikation mit übergeordneten Systemen**: Der Adapter ermöglicht die Anbindung an einen übergeordneten Zeitgeber-Baustein oder eine Steuerung, die den TimeTicker fernsteuern kann.
- **Zeitmessung und Überwachung**: Mit den Ausgaben `PT` (Prozesszeit) und `ET` (verstrichene Zeit) können Laufzeiten von Teilprozessen gemessen und überwacht werden.
- **Test und Simulation**: In Testumgebungen kann der TimeTicker verwendet werden, um zeitabhängige Testfälle zu generieren.

## Vergleich mit ähnlichen Bausteinen

- **E_CYCLE**: Der direkte Basisbaustein erzeugt periodische Ereignisse, besitzt aber keine Adapter-Schnittstelle und gibt keine Zeitinformationen aus. Der TimeTicker erweitert diese Funktionalität.
- **E_TIME** (falls vorhanden): Ein Baustein, der absolute Zeiten liefert, aber nicht für zyklische Ticks gedacht ist.
- **E_SWITCH**: Schaltet zwischen Eingängen um, hat aber keinen Zeitbezug.
- **Andere Ticker-Implementierungen**: Es gibt ähnliche Bausteine, die oft eigene Zeitbasis verwenden. Der Vorteil des TimeTickers liegt in der standardisierten `E_CYCLE`-Integration und der Adapter-Kopplung, die eine nahtlose Einbindung in bestehende IEC-61499-Systeme ermöglicht.

## Fazit

Der TimeTicker ist ein nützlicher, zusammengesetzter Funktionsblock, der die Erzeugung periodischer Ereignisse mit einer erweiterten Schnittstelle für Adapter-Kommunikation kombiniert. Er bietet eine saubere Trennung zwischen Zeitsteuerung und Anwendung, indem er über den Adapter eine flexible Ein- und Auskopplung von Steuersignalen und Zeitdaten ermöglicht. Durch die Verwendung des bewährten `E_CYCLE`-Bausteins ist die Zuverlässigkeit gegeben, und die Bereitstellung von `Q`, `PT` und `ET` erlaubt eine umfassende Überwachung der zeitlichen Abläufe. Der Baustein eignet sich daher besonders für den Einsatz in komplexen automatisierten Systemen, die eine präzise und rückgekoppelte Zeitsteuerung benötigen.
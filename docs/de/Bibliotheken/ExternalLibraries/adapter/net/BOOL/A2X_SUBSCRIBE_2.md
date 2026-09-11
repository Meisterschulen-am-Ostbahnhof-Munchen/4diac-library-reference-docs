# A2X_SUBSCRIBE_2

![A2X_SUBSCRIBE_2](./A2X_SUBSCRIBE_2.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **A2X_SUBSCRIBE_2** dient als Subscriber in einem industriellen Kommunikationsnetzwerk. Er empfängt zwei boolesche Werte von einem korrespondierenden **PUBLISH_2**-Block, puffert diese jeweils über ein D-Flipflop und stellt sie über einen unidirektionalen **A2X**-Adapter zur Verfügung. Dadurch können empfangene Signale stabil gehalten und über standardisierte Adapterschnittstellen an nachgelagerte Logik weitergegeben werden.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (EInit): Initialisierung des Bausteins. Wird typischerweise mit einem Initialisierungsereignis ausgelöst und setzt den Subscriber in einen definierten Zustand. Mit den Daten `QI` und `ID` verbunden.
- **RSP** (Event): Antwort auf ein zuvor gesendetes Request-Ereignis. Dient zur Quittierung von Kommunikationsvorgängen. Mit dem Daten-Eingang `QI` verbunden.

### **Ereignis-Ausgänge**

- **INITO** (EInit): Bestätigung der erfolgreichen Initialisierung. Wird zusammen mit den Daten `QO` und `STATUS` ausgegeben.
- **IND** (Event): Signalisiert, dass neue Daten vom Netzwerk empfangen wurden. Zusammen mit `QO` und `STATUS` ausgegeben.

### **Daten-Eingänge**

- **QI** (BOOL): Qualifizierer für die Initialisierung. Steuert den Initialisierungsvorgang.
- **ID** (WSTRING): Identifikator (z.B. Netzwerkadresse oder Topic) zur eindeutigen Zuordnung des Subscribers.

### **Daten-Ausgänge**

- **QO** (BOOL): Qualifizierer, ob der Baustein betriebsbereit ist.
- **STATUS** (WSTRING): Statusmeldung über den letzten Vorgang (z.B. Fehler oder Erfolg).

### **Adapter**

- **OUT** (Plug, Typ `adapter::types::unidirectional::A2X`): Unidirektionaler Ausgangs-Adapter. Stellt die beiden gepufferten booleschen Werte (`UP` und `DOWN`) sowie die zugehörigen Ereignisse (`E_UP`, `E_DOWN`) bereit.

## Funktionsweise

Der Baustein integriert intern einen **SUBSCRIBE_2**-Netzwerkbaustein (aus `iec61499::net`), der über die Eingänge `QI` und `ID` konfiguriert wird. Beim Eintreffen eines neuen Datensatzes (Ereignis `IND` des SUBSCRIBE_2) werden die beiden empfangenen Werte (`RD_1` und `RD_2`) jeweils an ein D-Flipflop (`E_D_FF_UP` und `E_D_FF_DOWN`) übergeben. Das Ereignis `IND` taktet gleichzeitig beide Flipflops, sodass die aktuellen Werte übernommen werden. Anschließend werden die Ausgänge der Flipflops über den Adapter `OUT` bereitgestellt. Die Adapter-Ereignisse `E_UP` und `E_DOWN` signalisieren, dass der jeweilige Wert aktualisiert wurde. Zusätzlich wird das `IND`-Ereignis des Subscribers direkt an den Ausgang `IND` weitergeleitet, sodass auch externe Bausteine über neue Daten informiert werden.

## Technische Besonderheiten

- **D-Flipflops zur Entkopplung**: Die zwei empfangenen BOOL-Werte werden zwischengespeichert, um kurze Signalpegel zu stabilisieren und eine saubere Übergabe über den Adapter zu gewährleisten.
- **Unidirektionaler Adapter**: Der Ausgangs-Adapter ist als Plug konzipiert, d.h. er gibt Daten nur aus und empfängt keine Rückmeldungen – ideal für reine Sensor-/Statusübertragungen.
- **Wiederverwendbarkeit**: Durch die Kapselung der Subscriber-Funktionalität und Pufferung in einem FB kann er leicht in verschiedene Automatisierungsprojekte integriert werden.
- **Ereignisgesteuerte Aktualisierung**: Die Flipflops werden nur bei einem neuen Datensatz getaktet (über `IND`), sodass keine unnötige CPU-Last entsteht.

## Zustandsübersicht

Der Baustein besitzt keinen expliziten Zustandsautomaten, folgt aber einem impliziten Ablauf:

1. **Initialisierung**: Über `INIT` wird der interne `SUBSCRIBE_2` mit `QI` und `ID` konfiguriert. Nach erfolgreicher Initialisierung wird `INITO` mit `QO=TRUE` und `STATUS` entsprechend ausgegeben.
2. **Betrieb**: Nach der Initialisierung lauscht der Baustein auf eingehende Daten. Bei jedem `IND`-Ereignis werden die neuen Werte in die Flipflops übernommen und über den Adapter ausgegeben.
3. **Fehlerzustand**: Falls die Kommunikation fehlschlägt, wird `STATUS` mit einer Fehlermeldung belegt und `QO` auf `FALSE` gesetzt.

## Anwendungsszenarien

- **Fernsteuerung von Aktoren**: Empfang von zwei booleschen Steuersignalen (z.B. Auf/Ab, Ein/Aus) von einer zentralen Steuerung und Weiterleitung an lokale Logik über den A2X-Adapter.
- **Statusüberwachung**: Übertragung von zwei Sensorzuständen (z.B. Grenzwertüberschreitung, Fehler) in ein lokales Leitsystem.
- **Modulare Automatisierung**: Einsatz in modularen Maschinenkonzepten, bei denen standardisierte Adapterschnittstellen für die Kommunikation zwischen verschiedenen Teilsystemen verwendet werden.

## Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einem einfachen `SUBSCRIBE_2`-Baustein, der die empfangenen Daten direkt an Datenausgänge weiterreicht, bietet `A2X_SUBSCRIBE_2` zusätzlich:

- **Zwischenspeicherung** der Werte über D-Flipflops, wodurch die Daten auch nach dem Empfang stabil bleiben.
- **Adapter-Schnittstelle** statt direkter Datenausgänge – dies erleichtert die Integration in bestehende Adapter-basierte Architekturen.
- **Separate Ereignisse** für jeden gepufferten Wert (`E_UP`, `E_DOWN`), was eine präzise Synchronisation mit nachgelagerten Bausteinen ermöglicht.
Gegenüber einer Lösung mit zwei getrennten Subscriber-Blöcken (einer pro BOOL) reduziert dieser Baustein den Konfigurationsaufwand und vereinheitlicht die Datenaufnahme.

## Fazit

Der Funktionsblock **A2X_SUBSCRIBE_2** stellt eine robuste und kompakte Lösung dar, um zwei boolesche Signale über ein Netzwerk zu empfangen und gepuffert über eine standardisierte Adapterschnittstelle bereitzustellen. Durch die Kombination von Subscriber-Funktionalität, D-Flipflop-Pufferung und Adapter-Integration bietet er eine hohe Wiederverwendbarkeit und erleichtert die Entwicklung verteilter Automatisierungssysteme. Insbesondere für Anwendungen, die eine stabile und ereignisgesteuerte Übergabe von Binärsignalen erfordern, ist dieser Baustein eine empfehlenswerte Wahl.

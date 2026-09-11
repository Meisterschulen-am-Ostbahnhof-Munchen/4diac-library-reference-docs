# ATimeTick

![ATimeTick](./ATimeTick.svg)

* * * * * * * * * *

## Einleitung

Der Adapter `ATimeTick` stellt eine standardisierte Schnittstelle für einen Zeitüberwachungsdienst (Timeout-Service) bereit, der sich an den Grunddefinitionen der ROOM-Methodik orientiert. Er ermöglicht die lose Kopplung zwischen einer steuernden Komponente und einer Zeitgeber- oder Überwachungsinstanz. Die Interaktion erfolgt über Ereignisse und Zeitparameter, wobei der Adapter sowohl als Plug- als auch als Socket-Typ eingesetzt werden kann.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Kommentar |
|----------|-----------|
| `CNF` | Bestätigung der Ausführung (Execution Confirmation), trägt die Daten `Q`, `ET` und `PT`. |
| `STARTO_IN` | Startanforderung von außen (ohne Parameter). |
| `STOPO_IN` | Stoppanforderung von außen (ohne Parameter). |

### **Ereignis-Ausgänge**

| Ereignis | Kommentar |
|----------|-----------|
| `REQ` | Normale Ausführungsanforderung (Normal Execution Request). |

### **Daten-Eingänge**

| Variable | Typ   | Kommentar                       |
|----------|-------|---------------------------------|
| `Q`      | BOOL  | Zeigt an, ob der Timer aktiv ist (`TRUE` = gestartet). |
| `PT`     | TIME  | Prozesszeit (Sollwert für die Zeitüberwachung). |
| `ET`     | TIME  | Verstrichene Zeit (Istwert).    |

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

Der Adapter definiert zwei Service-Sequenzen:

- **`Timeout`**: Über die Plug-Seite wird das Ereignis `START` mit Parameter `TD` gesendet und auf der Socket-Seite weitergegeben. Anschließend wird von der Socket-Seite das Ereignis `TimeOut` empfangen und auf der Plug-Seite weitergeleitet.
- **`NormalOperation`**: Die Plug-Seite sendet `Start` (mit `TD`) und `STOP` (ohne Parameter) an die Socket-Seite.

Die Zuordnung der Ereignisse zu den Service-Sequenzen zeigt die erwartete Kommunikationsreihenfolge. Der Adapter unterstützt damit sowohl das Starten eines Timers als auch dessen Abbruch sowie die Rückmeldung eines Timeouts.

## Funktionsweise

Der `ATimeTick`-Adapter dient als Vermittler zwischen einer Anwendungskomponente und einer Zeitüberwachungslogik. Die Steuerung kann über die Plug-Seite den Timer mit einer vorgegebenen Zeit (`TD` bzw. `PT`) starten. Die Zeitüberwachung (Socket-Seite) bestätigt den Start und liefert nach Ablauf einen Timeout-Ereignis zurück. Durch die ereignisgesteuerte Kommunikation wird die Zeitlogik von der eigentlichen Anwendung entkoppelt.

Die Daten `Q`, `ET` und `PT` werden über das `CNF`-Ereignis übertragen, das die Bestätigung der Ausführung darstellt. Über `Q` wird der aktive Zustand signalisiert, `ET` enthält die verstrichene Zeit und `PT` die eingestellte Prozesszeit. Damit stehen der Anwendung alle relevanten Informationen zur Verfügung.

## Technische Besonderheiten

- **Ereignis- und Datenkopplung**: Beim `CNF`-Ereignis sind die Daten `Q`, `ET` und `PT` mitgebunden, was eine effiziente und synchrone Übertragung ermöglicht.
- **Plug/Socket-Prinzip**: Der Adapter kann wahlweise als Plug oder Socket verwendet werden, wodurch eine flexible Verschaltung in unterschiedlichen Kontexten möglich ist.
- **Service-Sequenzen**: Die definierten Sequenzen `Timeout` und `NormalOperation` beschreiben den erwarteten Nachrichtenaustausch und unterstützen die Verifikation und Implementierung.
- **Keine Datenausgänge**: Der Adapter kommuniziert primär über Ereignisse, Daten werden nur als Eingänge für die Bestätigung verwendet.

## Zustandsübersicht

Der Adapter selbst besitzt keinen internen Zustandsautomaten, da er rein als Schnittstellentyp fungiert. Die möglichen Abläufe können jedoch aus den Service-Sequenzen abgeleitet werden:

- **Startzustand**: Über `START`/`Start` wird der Timer initialisiert.
- **Laufzustand**: Nach dem Start ist der Timer aktiv (`Q = TRUE`), die Zeit läuft.
- **Stoppzustand**: Über `STOP` wird der Timer vorzeitig beendet.
- **Timeout-Zustand**: Nach Ablauf der Zeit wird `TimeOut` ausgelöst, und die Anwendung erhält das Ereignis.

Die Zustandsübergänge sind ereignisgesteuert und hängen von der implementierenden Logik ab.

## Anwendungsszenarien

- **Zeitüberwachung in industriellen Steuerungen**: Einsatz in SPS-Programmen zur Überwachung von Prozesszeiten, z.B. für Last- oder Laufzeitüberwachung.
- **Wartungsintervalle**: Überwachung von Wartungszeiten oder Bearbeitungsfristen in Automatisierungssystemen.
- **Ereignisgesteuerte Zeitgeber**: In verteilten Systemen kann der Adapter verwendet werden, um Zeitgeberfunktionen über verschiedene Komponenten hinweg zu synchronisieren.

## Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einem klassischen `TON`-Funktionsblock (Timer ON-Delay) bietet der `ATimeTick`-Adapter eine isolierte Schnittstelle, die vom eigentlichen Zeitverhalten entkoppelt ist. Während ein FB die Zeitlogik direkt implementiert, definiert der Adapter nur die Kommunikationsprotokolle. Dadurch eignet er sich für modulare und wiederverwendbare Architekturen, bei denen die Zeitlogik austauschbar sein soll.

## Fazit

Der `ATimeTick`-Adapter ist eine elegante Lösung zur Integration von Zeitüberwachungsdiensten in ereignisbasierte Automatisierungssysteme. Durch die klare Trennung von Schnittstelle und Implementierung ermöglicht er eine flexible und wartungsfreundliche Systemarchitektur. Die definierten Service-Sequenzen erleichtern die korrekte Einbindung und Kommunikation, insbesondere in Umgebungen mit verteilten Komponenten.

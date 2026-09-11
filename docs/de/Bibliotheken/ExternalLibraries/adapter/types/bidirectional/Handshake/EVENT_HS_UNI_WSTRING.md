# EVENT_HS_UNI_WSTRING

![EVENT_HS_UNI_WSTRING](./EVENT_HS_UNI_WSTRING.svg)

* * * * * * * * * *
## Einleitung

Der Adapter `EVENT_HS_UNI_WSTRING` gehört zur `EVENT_HS`-Familie (Handshake-Designmuster nach IEC 61499) und stellt eine reduzierte, unidirektionale Variante dar. Er dient der Übertragung einer „Fire-and-Forget“-Nachricht (ohne Antwort) mit einem WSTRING-Payload vom Plug zum Socket. Der Name „UNI“ weist darauf hin, dass nur eine Richtung (ein Ereignis) existiert – es gibt weder Bestätigungen noch Rückkanäle. Diese Variante erweitert `EVENT_HS_UNI` um ein Datenfeld `REQD`, das zusammen mit dem Ereignis `REQ` gesendet wird.

Der Adapter ist für Szenarien gedacht, in denen eine einseitige, best-effort Benachrichtigung mit einem String-Payload ausreicht und keine Rückmeldung über Empfang oder Verarbeitung erforderlich ist.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Es sind keine Ereignis-Eingänge vorhanden.

### **Ereignis-Ausgänge**

| Name | Typ   | Mit Variablen | Kommentar                                  |
|------|-------|---------------|--------------------------------------------|
| REQ  | Event | REQD          | Anforderung/Benachrichtigung vom Plug zum Socket; keine Antwort erwartet. |

### **Daten-Eingänge**

Es sind keine Daten-Eingänge vorhanden.

### **Daten-Ausgänge**

| Name | Typ     | Kommentar                                      |
|------|---------|------------------------------------------------|
| REQD | WSTRING | Nutzlast, die zusammen mit dem Ereignis `REQ` übertragen wird. |

### **Adapter**

Der Baustein ist selbst ein Adapter und besitzt daher keine weiteren Adapter-Schnittstellen.

## Funktionsweise

Der Adapter realisiert eine einfache, unidirektionale Ereignisübertragung: Der Plug (linke Schnittstelle) erzeugt das Ereignis `REQ` und übergibt dabei den Wert von `REQD` als Parameter. Der Socket (rechte Schnittstelle) empfängt dieses Ereignis und erhält die zugehörige Nutzlast.

Die Service-Sequenz `notify` im XML beschreibt diesen Ablauf explizit als eine Transaktion:

- **Input**: `PLUG` → Ereignis `REQ` mit Parameter `REQD`
- **Output**: `SOCKET` → Ereignis `REQ` mit Parameter `REQD`

Es existiert keinerlei Rückkanal; weder eine Bestätigung (`CNF`), noch eine Anzeige (`IND`) oder eine Antwort (`RSP`). Der Adapter leitet das Ereignis einfach vom Plug zum Socket weiter.

## Technische Besonderheiten

- **Kein echter Handshake**: Wie der Name andeutet, handelt es sich um eine „Fire-and-Forget“-Variante. Der Socket hat keine Möglichkeit, die Anfrage oder deren Nutzlast zu bestätigen oder abzulehnen. Der Plug kann nicht feststellen, ob der Socket das Ereignis erhalten oder verarbeitet hat.
- **Datenübertragung**: Die Nutzlast `REQD` ist vom Typ `WSTRING` und wird nur zusammen mit `REQ` übertragen. Es gibt keine separate Datenverbindung.
- **Dokumentarischer Service**: Der `<Service>`-Block im XML ist rein informativ (gemäß XSD optional) und nicht erforderlich, damit die Adapterverbindung Ereignisse weiterleitet. Er wurde dennoch hinzugefügt, um die einzige Transaktion zu verdeutlichen.
- **Rollensplit**: Wie bei allen Adaptoren der `EVENT_HS`-Familie besitzt der Plug die aktive Rolle (Sender), der Socket die passive Rolle (Empfänger). Die Richtung ist im Adaptertyp festgelegt.

## Zustandsübersicht

Der Adapter besitzt keine internen Zustände. Er ist eine reine „Durchreiche“ für das Ereignis `REQ` und dessen Nutzlast `REQD`. Es gibt genau eine Übertragung pro Aufruf, ohne Verzögerung oder Zwischenspeicherung.

## Anwendungsszenarien

- **Push-Nachrichten**: Senden von Benachrichtigungen oder Statusmeldungen, bei denen keine Bestätigung erforderlich ist (z. B. Log-Einträge, Sensorwerte an einen Anzeigeprozess).
- **Ereignisbasierte Datenweitergabe**: Übertragung eines Strings als Teil eines Ereignisses, wenn nur eine Richtung kommuniziert wird.
- **Integration in bestehende Handshake-Strukturen**: Als Alternative zu `EVENT_HS_UNI`, wenn zusätzlich ein WSTRING-Payload benötigt wird, aber weiterhin auf Rückkanäle verzichtet werden kann.

## Vergleich mit ähnlichen Bausteinen

Innerhalb der `EVENT_HS`-Familie gibt es mehrere Varianten:

| Baustein                 | Richtung | Payload      | Bestätigung | Zweck                                      |
|--------------------------|----------|--------------|-------------|--------------------------------------------|
| `EVENT_HS`               | bidirektional | –       | ja          | Vollständiger Handshake (REQ/CNF, IND/RSP) |
| `EVENT_HS_UNI`           | unidirektional | –       | nein        | Fire-and-Forget-Ereignis ohne Nutzlast     |
| `EVENT_HS_ACK`           | unidirektional | –       | ja          | Fire-and-Forget mit Bestätigung (ACK)      |
| `EVENT_HS_ACK_WSTRING`   | unidirektional | WSTRING | ja          | Fire-and-Forget mit Payload und Bestätigung |
| `EVENT_HS_UNI_WSTRING`   | unidirektional | WSTRING | nein        | Fire-and-Forget mit Payload ohne Bestätigung |

Im Vergleich zu `EVENT_HS_UNI` fügt dieser Baustein die Nutzlast `REQD` hinzu. Gegenüber `EVENT_HS_ACK_WSTRING` entfällt die Bestätigung, wodurch der Kommunikationsaufwand minimal bleibt, aber auch keine Erfolgskontrolle existiert.

## Fazit

`EVENT_HS_UNI_WSTRING` ist ein schlanker Adapter für unidirektionale, best-effort Ereignisübertragungen mit einem String-Payload. Er eignet sich ausschließlich für Szenarien, in denen eine fehlende Rückmeldung akzeptabel ist – etwa bei reinen Benachrichtigungen oder Statusmeldungen. Aufgrund der fehlenden Bestätigungsmechanismen sollte er nicht verwendet werden, wenn eine verlässliche Zustellung oder Verarbeitung überwacht werden muss. In solchen Fällen ist eine Variante mit Bestätigung (z. B. `EVENT_HS_ACK_WSTRING`) vorzuziehen.
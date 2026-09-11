# EVENT_HS_ACK

![EVENT_HS_ACK](./EVENT_HS_ACK.svg)

* * * * * * * * * *
## Einleitung

Der Adapter `EVENT_HS_ACK` ist eine reduzierte Variante des Handshake-Adapters `EVENT_HS` aus dem IEC-61499-Entwurfsmuster. Er implementiert ausschließlich die Request-Confirm-Hälfte (REQ/CNF) der vollständigen Handshake-Vokabulars (REQ/CNF/IND/RSP). Damit wird eine zuverlässige Auftragsbestätigung ohne Datenübertragung realisiert – der Socket (Responder) kann auf eine Request (REQ) nur mit einer Confirmation (CNF) antworten, besitzt aber keine Möglichkeit, unaufgefordert Indikationen an den Plug (Requester) zu senden. Dies macht den Adapter ideal für einfache Steuerungs- und Synchronisationsaufgaben, bei denen lediglich eine positive Quittung auf eine Anforderung benötigt wird.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ   | Kommentar                               |
|------|-------|------------------------------------------|
| CNF  | Event | Bestätigung vom Socket zum Plug, beantwortet eine REQ |

### **Ereignis-Ausgänge**

| Name | Typ   | Kommentar                               |
|------|-------|------------------------------------------|
| REQ  | Event | Anforderung (Request) vom Plug zum Socket |

### **Daten-Eingänge**

Keine – der Adapter ist datenlos.

### **Daten-Ausgänge**

Keine – der Adapter ist datenlos.

### **Adapter**

Keine – der Baustein ist selbst ein Adapter (Plug/Socket).

## Funktionsweise

Der Adapter definiert eine einfache Handshake-Sequenz mit zwei Transaktionen:

1. Der Plug (Requester) sendet ein `REQ`-Ereignis über die linke Schnittstelle. Dieses wird unmittelbar als `REQ`-Ereignis an der rechten Schnittstelle (Socket) ausgegeben.
2. Der Socket (Responder) beantwortet die Anforderung mit einem `CNF`-Ereignis. Dieses wird vom Adapter als `CNF`-Ereignis an die linke Schnittstelle (Plug) weitergeleitet.

Somit erhält jede `REQ`-Anforderung eine zugehörige `CNF`-Bestätigung. Die Rollenverteilung folgt dem Standard des `EVENT_HS`-Adapters: Der Plug agiert als Client/Requester, der Socket als Server/Responder. Der Adapter besitzt keine interne Zustandslogik – er dient lediglich als ereignisbasierte Verbindung und ermöglicht eine lose Kopplung zwischen Komponenten.

## Technische Besonderheiten

- **Datenlos**: Es werden keine Daten übertragen – nur Ereignisse. Dadurch wird der Adapter besonders schlank und effizient.
- **Reduziertes Vokabular**: Im Gegensatz zum vollständigen `EVENT_HS` fehlen die Ereignisse `IND` (Indikation) und `RSP` (Response). Der Socket kann also nie unaufgefordert etwas an den Plug melden; er reagiert ausschließlich auf `REQ`.
- **Design-Pattern-Herkunft**: Der Adapter gehört zur Handshake-Familie und ist in einem separaten Dokument (`HandshakePattern.md`) detailliert beschrieben.
- **Eindeutige Schnittstellenrollen**: Durch die klare Trennung von Plug und Socket wird die Verbindungsrichtung im System explizit festgelegt.

## Zustandsübersicht

Da der Adapter keine eigenen Zustände besitzt, gibt es keine Zustandsautomaten im Inneren. Die Kommunikation erfolgt rein ereignisgesteuert. Im Rahmen einer Verbindung können jedoch die folgenden Phasen unterschieden werden:

- **Idle**: Es liegt kein Ereignis an.
- **Warten auf Bestätigung**: Nach dem Senden von `REQ` wartet der Plug auf die `CNF`-Antwort.
- **Bestätigt**: Der Plug hat die `CNF` erhalten; die Transaktion ist abgeschlossen.

Diese Phasen werden nicht vom Adapter selbst verwaltet, sondern durch die angeschlossenen Funktionsbausteine.

## Anwendungsszenarien

- **Steuerungsquittungen**: Eine SPS sendet einen Befehl an einen Aktor und wartet auf die Bestätigung, dass der Befehl ausgeführt wurde.
- **Synchronisation**: Zwei parallele Prozesse sollen sich gegenseitig per Request/Confirm abstimmen, ohne Daten auszutauschen.
- **Subsystem-Initialisierung**: Ein übergeordnetes System fordert ein Subsystem zum Start auf und erhält eine Bestätigung, sobald der Startvorgang abgeschlossen ist.
- **Redundanz**: In Verbindung mit einer Automatisierungskomponente kann `EVENT_HS_ACK` eine einfache, zuverlässige Übermittlung von Quittungen gewährleisten.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Ereignisse | Datentyp | Beschreibung |
|----------|------------|----------|--------------|
| `EVENT_HS` | REQ, CNF, IND, RSP | – (datenlos) | Vollständiger Handshake mit Indikation/Response |
| `EVENT_HS_UNI` | REQ, CNF | – (datenlos) | Unidirektionaler Handshake (nur Anforderung/Bestätigung) |
| `EVENT_HS_ACK` | REQ, CNF | – (datenlos) | Wie `EVENT_HS_UNI`, aber jede REQ erhalten eine CNF (aktive Bestätigung) |
| `EVENT_HS_UNI_WSTRING` | REQ, CNF | WSTRING | Unidirektionaler Handshake mit Datenübertragung |
| `EVENT_HS_ACK_WSTRING` | REQ, CNF | WSTRING | Handshake mit Daten und garantierter Bestätigung |

Im Vergleich zu `EVENT_HS` ist `EVENT_HS_ACK` auf die minimal erforderliche Kommunikation reduziert. Gegenüber `EVENT_HS_UNI` stellt `EVENT_HS_ACK` sicher, dass jede Anforderung tatsächlich quittiert wird – eine einfache, aber wichtige Erweiterung für Systeme, die eine positive Rückmeldung verlangen.

## Fazit

`EVENT_HS_ACK` ist ein schlanker, datenloser Adapter für eine klar umrissene Aufgabe: eine Anforderung senden und eine Bestätigung empfangen. Er eignet sich hervorragend für Anwendungen, die eine robuste, ereignisbasierte Quittung benötigen, ohne den Overhead eines vollständigen Handshakes oder einer Datenübertragung. Durch die klare Plug/Socket-Rollenverteilung und die garantierten REQ-CNF-Paare bietet er eine einfache, aber zuverlässige Grundlage für die Kommunikation zwischen IEC-61499-Komponenten.
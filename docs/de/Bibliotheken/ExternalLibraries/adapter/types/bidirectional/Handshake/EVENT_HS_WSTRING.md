# EVENT_HS_WSTRING

![EVENT_HS_WSTRING](./EVENT_HS_WSTRING.svg)

* * * * * * * * * *

## Einleitung

Der Adapter `EVENT_HS_WSTRING` ist eine datentragende Variante des bekannten **Handshake-Entwurfsmusters** für IEC 61499-Applikationen. Er ergänzt das klassische `EVENT_HS`-Adapter-Interface um eine `WSTRING`-Nutzlast pro Ereignis, sodass bei jedem Handshake-Schritt zusätzliche Informationen übertragen werden können. Das Muster ermöglicht eine zuverlässige, sequenzielle Kommunikation zwischen einem Client (Plug) und einem Server (Socket) und eignet sich besonders für Service-orientierte Architekturen und Prozessdaten-Schnittstellen.

Der Adapter basiert auf dem IEC 61499-Standard und ist für die Verwendung in der 4diac-IDE konzipiert. Er implementiert die Rollenverteilung **Plug** (Client/Requester) und **Socket** (Server/Responder) und unterstützt die beiden Service-Sequenzen *request_confirm* und *indication_response*. Die Datenfelder `CNFD`, `INDD`, `REQD` und `RSPD` transportieren jeweils die zugehörige Payload.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Datentyp | Kommentar |
|----------|----------|-----------|
| `CNF`    | Event    | Bestätigung vom Socket zum Plug; beantwortet eine `REQ`-Anfrage. |
| `IND`    | Event    | Unaufgeforderte Anzeige vom Socket zum Plug. |

### **Ereignis-Ausgänge**

| Ereignis | Datentyp | Kommentar |
|----------|----------|-----------|
| `REQ`    | Event    | Anforderung vom Plug zum Socket. |
| `RSP`    | Event    | Antwort vom Plug zum Socket; beantwortet eine `IND`. |

### **Daten-Eingänge**

| Variable | Datentyp | Kommentar |
|----------|----------|-----------|
| `CNFD`   | `WSTRING`| Nutzlast, die mit dem `CNF`-Ereignis übertragen wird. |
| `INDD`   | `WSTRING`| Nutzlast, die mit dem `IND`-Ereignis übertragen wird. |

### **Daten-Ausgänge**

| Variable | Datentyp | Kommentar |
|----------|----------|-----------|
| `REQD`   | `WSTRING`| Nutzlast, die mit dem `REQ`-Ereignis übertragen wird. |
| `RSPD`   | `WSTRING`| Nutzlast, die mit dem `RSP`-Ereignis übertragen wird. |

### **Adapter**

Der Adapter besitzt zwei Schnittstellen:

- **Plug** (links): Dies ist der Client/Requester. Er sendet `REQ` und `RSP` und empfängt `CNF` und `IND`.
- **Socket** (rechts): Dies ist der Server/Responder. Er empfängt `REQ` und `RSP` und sendet `CNF` und `IND`.

Die Zuordnung erfolgt über die Service-Definition, in der der Plug als `LeftInterface` und der Socket als `RightInterface` festgelegt sind.

## Funktionsweise

Der Adapter realisiert zwei unabhängige Handshake-Sequenzen, die über die Service-Sequenzen definiert werden:

1. **request_confirm** – Dies ist der klassische Request-Confirm-Zyklus:
   - Der Plug sendet ein `REQ`-Ereignis zusammen mit der Nutzlast `REQD` an den Socket.
   - Der Socket empfängt dieses Ereignis und verarbeitet die Daten.
   - Anschließend sendet der Socket ein `CNF`-Ereignis mit `CNFD` zurück an den Plug, um die Ausführung zu bestätigen.

2. **indication_response** – Dies ermöglicht eine Server-initiierte Kommunikation:
   - Der Socket sendet unaufgefordert ein `IND`-Ereignis mit `INDD` an den Plug (z. B. zur Signalisierung eines Ereignisses).
   - Der Plug empfängt dies und antwortet mit einem `RSP`-Ereignis samt `RSPD` an den Socket.

Beide Sequenzen sind vollständig getrennt und können unabhängig voneinander ablaufen. Die Datenvariablen sind jeweils an das zugehörige Ereignis gebunden und werden mit dem Ereignis übertragen. Dadurch ist sichergestellt, dass die Nutzlast immer im korrekten Kontext ankommt.

## Technische Besonderheiten

- **Datenvariante des Handshake-Musters**: Im Vergleich zum datenlosen `EVENT_HS`-Adapter trägt jedes Ereignis eine `WSTRING`-Nutzlast. Dies entspricht dem generischen *Service*-Adapter aus dem Lehrbuch von Valeriy Vyatkin (IEC 61499 Primer, Modul 6, Folie 48).
- **Fester Datentyp**: Die Payload ist aktuell auf `WSTRING` festgelegt. Bei Bedarf kann eine typspezifischere Variante abgeleitet werden.
- **Eindeutige Rollentrennung**: Plug und Socket haben klar definierte Aufgaben; dies erleichtert die Wiederverwendung in verschiedenen Applikationen.
- **Paketzuordnung**: Der Adapter ist im Paket `adapter::types::bidirectional::Handshake` organisiert, was eine saubere Einbindung in größere Projekte ermöglicht.
- **Versionierung**: Der Adapter ist mit Version 1.0 und dem Autor Franz Höpfinger (HR Agrartechnik GmbH) hinterlegt. Die Historie enthält Verweise auf das Vorlesungsmaterial und die zugehörige Dokumentation.

## Zustandsübersicht

Der Adapter selbst besitzt keinen expliziten Zustandsautomaten, die beiden Service-Sequenzen definieren jedoch klare Zustandsübergänge:

| Sequenzname | Schritt | Ereignis / Richtung | Beschreibung |
|-------------|---------|---------------------|--------------|
| `request_confirm` | 1 | REQ (Plug → Socket) | Plug sendet Anforderung und Nutzlast. |
| `request_confirm` | 2 | CNF (Socket → Plug) | Socket bestätigt die Ausführung. |
| `indication_response` | 1 | IND (Socket → Plug) | Socket sendet eine unaufgeforderte Anzeige. |
| `indication_response` | 2 | RSP (Plug → Socket) | Plug quittiert die Anzeige. |

In der Praxis bedeutet dies: Nach dem Senden eines `REQ` wartet der Plug auf ein `CNF`; nach dem Empfang eines `IND` muss der Plug mit `RSP` antworten. Es gibt keine parallelen Überlappungen innerhalb einer Sequenz, wodurch eine deterministische Kommunikation gewährleistet ist.

## Anwendungsszenarien

- **Service-Aufrufe in industriellen Steuerungen**: Ein Steuerungsgerät (Plug) sendet einen Befehl wie `"push,100"` an ein Servicemodul (Socket) und erhält eine Bestätigung.
- **Status- und Ereignisbenachrichtigungen**: Ein Sensor (Socket) sendet unaufgefordert eine Messwertänderung (`IND`) an die Steuerung (Plug), die daraufhin eine Quittung (`RSP`) zurücksendet.
- **Parametrierung und Konfiguration**: Der Adapter eignet sich für den Austausch von Konfigurationsdaten zwischen Engineering-Tool und Laufzeitsystem, wenn eine asynchrone, aber zuverlässige Übertragung gefordert ist.
- **Bildung von Subsystemen**: Innerhalb von Subapplikationen kann der Adapter als klar definierte Schnittstelle zwischen Modulen verwendet werden, um lose Kopplung und Wiederverwendbarkeit zu erhöhen.

## Vergleich mit ähnlichen Bausteinen

| Adapter | Ereignisse | Nutzlast | Zweck |
|---------|------------|----------|-------|
| `EVENT_HS` | REQ, CNF, IND, RSP | – | Reines Handshake-Muster ohne Daten, ideal für einfache Synchronisation. |
| `EVENT_HS_WSTRING` | REQ, CNF, IND, RSP | `WSTRING` pro Ereignis | Datenhandshake für Text-basierte Nachrichten (z. B. Befehle, Status). |
| `PUBLISH/SUBSCRIBE`-Adapter | Unterschiedlich | Beliebig | Publish-Subscribe-Muster, eher für unidirektionale Kommunikation. |

Der Vorteil von `EVENT_HS_WSTRING` gegenüber `EVENT_HS` ist die direkte Mitnahme von Informationen in beiden Richtungen, ohne zusätzliche Datenkanäle aufbauen zu müssen. Gegenüber generischen Publish/Subscribe-Mechanismen bietet er die Vorteile eines strengen Handshake-Protokolls (Request-Confirm und Indication-Response), was für viele Steuerungsanwendungen robuster ist.

## Fazit

Der Adapter `EVENT_HS_WSTRING` stellt eine praxisgerechte Erweiterung des Handshake-Musters dar. Durch die Integration einer `WSTRING`-Nutzlast in jedes Ereignis wird die ursprünglich reine Synchronisationslogik zu einem vollwertigen Kommunikationsmittel für textuelle Befehle, Statusmeldungen oder Konfigurationsdaten. Die klare Trennung zwischen Plug und Socket sowie die beiden sauber definierten Service-Sequenzen machen den Adapter zu einer zuverlässigen Wahl für serviceorientierte Architekturen in IEC 61499-Anwendungen. Dank der eindeutigen Versionierung und der dokumentierten Herkunft ist er zudem gut nachvollziehbar und wartbar. Für den Einsatz in Projekten, die eine reine Datenübertragung ohne Handshake-Anforderungen benötigen, sollte man aber auf andere Muster wie Publish/Subscribe zurückgreifen. Insgesamt bietet `EVENT_HS_WSTRING` einen guten Kompromiss zwischen Einfachheit und Funktionsumfang.

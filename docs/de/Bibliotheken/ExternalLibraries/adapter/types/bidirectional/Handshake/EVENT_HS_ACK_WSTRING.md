# EVENT_HS_ACK_WSTRING

![EVENT_HS_ACK_WSTRING](./EVENT_HS_ACK_WSTRING.svg)

* * * * * * * * * *
## Einleitung

Der Adapter `EVENT_HS_ACK_WSTRING` gehört zur Familie der Handshake-Adapter (nach dem IEC 61499-Muster). Er realisiert eine reduzierte Variante des klassischen `EVENT_HS`-Adapters, bei der nur die **Request-/Confirm-Kante** (REQ/CNF) verwendet wird. Beide Ereignisse transportieren jeweils einen Datenparameter vom Typ `WSTRING`. Dadurch eignet sich dieser Adapter für Anwendungen, in denen eine Anfrage und eine Bestätigung mit einer textuellen Nutzlast (z. B. im Format `"name,value"`) ausgetauscht werden müssen.

Der Adapter stellt eine Plug-/Socket-Schnittstelle bereit: Die **Plug**-Seite (links) agiert als Requester/Client, die **Socket**-Seite (rechts) als Responder/Server. Es gibt keine unaufgeforderten Indications (IND) oder Responses (RSP) – die Kommunikation folgt strikt dem Muster: Plug sendet REQ mit REQD, Socket antwortet mit CNF und CNFD.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **CNF** (Confirmation)  
  Wird von der Socket-Seite ausgelöst, um eine zuvor gesendete REQ-Anfrage zu bestätigen.  
  *Mit Datenparameter:* `CNFD` (WSTRING) – die Bestätigungsnutzlast.

### **Ereignis-Ausgänge**

- **REQ** (Request)  
  Wird von der Plug-Seite ausgelöst, um eine Anfrage an die Socket-Seite zu senden.  
  *Mit Datenparameter:* `REQD` (WSTRING) – die Anfragenutzlast.

### **Daten-Eingänge**

- **CNFD** (WSTRING) – Begleitdaten zum Ereignis `CNF`. Enthält die Bestätigungsinformation (z. B. `"push,100"`), die vom Socket an den Plug zurückgegeben wird.

### **Daten-Ausgänge**

- **REQD** (WSTRING) – Begleitdaten zum Ereignis `REQ`. Enthält die Anfrageinformation (z. B. `"push,100"`), die vom Plug an den Socket gesendet wird.

### **Adapter**

Der Adapter selbst definiert eine bidirektionale Schnittstelle mit zwei Rollen:

- **PLUG** (links) – kontrolliert die Richtung der Ereignisse: REQ wird **gesendet**, CNF wird **empfangen**.  
- **SOCKET** (rechts) – spiegelbildlich: REQ wird **empfangen**, CNF wird **gesendet**.

Die Zuordnung erfolgt beim Verbinden zweier FB-Instanzen, wobei der Adapter entweder als **Plug** oder als **Socket** verdrahtet wird.

## Funktionsweise

Der Adapter implementiert einen einfachen Request-Confirm-Handshake mit textuellen Nutzlasten. Die Abfolge ist wie folgt:

1. **Plug** setzt den Ausgangsdatenwert `REQD` und sendet das Ereignis **REQ**.
2. **Socket** empfängt das Ereignis **REQ** und den zugehörigen Wert `REQD`.
3. **Socket** verarbeitet die Anfrage und setzt den Eingangswert `CNFD` (bei der Socket-Instanz) und löst das Ereignis **CNF** aus.
4. **Plug** empfängt das Ereignis **CNF** und den Wert `CNFD`.

Wichtig: Es gibt **keine Indication (IND)** und **keine Response (RSP)**. Das bedeutet, dass die Socket-Seite nicht von sich aus eine unaufgeforderte Nachricht an den Plug senden kann – jede Kommunikation muss mit einem REQ des Plugs beginnen und wird durch ein CNF des Sockets abgeschlossen.

## Technische Besonderheiten

- **Nur Request/Confirm-Pfad** – bewusste Reduktion des vollen Handshake-Vokabulars (REQ, CNF, IND, RSP) auf die beiden Ereignisse `REQ` und `CNF`.
- **WSTRING-Nutzlast** – beide Ereignisse sind mit einem Datenparameter vom Typ `WSTRING` verknüpft, was die Übertragung beliebiger Textdaten (z. B. Kommando-Strings) ermöglicht.
- **Klare Rollenverteilung** – Plug ist immer der Initiator, Socket der Responder. Dies erleichtert die Implementierung deterministischer Kommunikationsprotokolle.
- **Einfaches Dienstmodell** – Die Service-Sequenz `request_confirm` definiert exakt eine Transaktionskette, sodass keine zusätzlichen Zustände oder Nebenläufigkeiten behandelt werden müssen.

## Zustandsübersicht

Der Adapter besitzt keinen expliziten internen Zustandsautomaten – der Protokollablauf ist durch die Service-Sequenz festgelegt. Es lassen sich jedoch zwei logische Phasen unterscheiden:

| Phase | Aktivität |
|-------|-----------|
| **Warten auf REQ** | Plug kann einen neuen REQ senden. Sobald REQ gesendet wird, wartet der Plug auf das zugehörige CNF. |
| **Warten auf CNF** | Socket hat REQ empfangen und arbeitet die Anfrage ab. Nach Abschluss sendet Socket das CNF. Danach ist der Zyklus abgeschlossen und ein neuer REQ kann beginnen. |

## Anwendungsszenarien

- **Kommandosteuerung**: Senden von Befehlen wie `"push,100"` an einen Server und Quittierung mit demselben oder einem modifizierten Wert.
- **Parameterabfrage**: Anfordern eines bestimmten Werts per REQ und Empfang des Ergebnisses im CNF.
- **Einfache Synchronisation**: Sicherstellen, dass eine Aktion nur nach erfolgreicher Bestätigung ausgeführt wird.
- **Bidirektionale Punkt-zu-Punkt-Kommunikation** in IEC-61499-Systemen, bei der keine unaufgeforderten Meldungen benötigt werden.

## Vergleich mit ähnlichen Bausteinen

- **`EVENT_HS`** – Vollständiger Handshake mit REQ/CNF und IND/RSP. `EVENT_HS_ACK_WSTRING` ist eine reduzierte Variante ohne IND/RSP, dafür mit WSTRING-Payload.
- **`EVENT_HS_ACK`** – Bietet ebenfalls nur REQ/CNF, jedoch **ohne** Datenparameter. `EVENT_HS_ACK_WSTRING` erweitert diesen um die Übertragung von Nutzdaten.
- **`EVENT_HS_UNI`** – Verwendet nur REQ/CNF (ohne Daten) und ist für unidirektionale Bestätigungen gedacht.
- **`EVENT_HS_UNI_WSTRING`** – Ähnlich wie `EVENT_HS_UNI`, aber mit WSTRING-Payload (ohne CNF-Daten). Im Gegensatz dazu besitzt `EVENT_HS_ACK_WSTRING` auch Daten beim CNF, sodass eine echte Quittung mit Inhalt möglich ist.

## Fazit

`EVENT_HS_ACK_WSTRING` ist ein kompakter und klar definierter Adapter für den IEC-61499-Handshake mit textueller Nutzlast. Durch die Reduktion auf Request/Confirm wird die Kommunikation deterministisch und leicht verständlich. Die Integration von WSTRING-Daten macht ihn flexibel für verschiedenste Anwendungen, in denen eine Anfrage mit einer Bestätigung inklusive Rückmeldung verbunden werden soll. Seine einfache Struktur erleichtert die Wiederverwendung und die Einbindung in größere Systeme.
# EVENT_HS

![EVENT_HS](./EVENT_HS.svg)

* * * * * * * * * *
## Einleitung
Der **EVENT_HS**-Adapter realisiert das **Handshake-Muster** aus dem IEC 61499 Primer (Modul 6 – Designmethoden und -muster, Valeriy Vyatkin). Er bündelt die vier klassischen Service-Primitive *Request*, *Confirm*, *Indication* und *Response* in einer einzigen Adapter-Verbindung und verzichtet dabei vollständig auf Datenvariablen (dataless). Dadurch wird eine reine Ereignissignalisierung zwischen zwei Kommunikationspartnern ermöglicht, ohne dass separate Ereignisverbindungen oder zusätzliche Datenleitungen benötigt werden.

Der Adapter definiert zwei Rollen: den **Plug** (links, Requester/Client) und den **Socket** (rechts, Responder/Server). Der Plug sendet REQ/RSP und empfängt CNF/IND, während der Socket genau umgekehrt arbeitet. Diese Zuordnung entspricht der natürlichen Leserichtung im FBNetwork-Editor und ist seit Version 1.1 des Adapters festgelegt.

## Schnittstellenstruktur
### **Ereignis-Eingänge**
- **CNF** – *Confirmation*: Bestätigung vom Socket an den Plug, antwortet auf eine REQ-Anfrage.  
- **IND** – *Indication*: Unaufgeforderte Meldung vom Socket an den Plug (z. B. über ein extern aufgetretenes Ereignis).

### **Ereignis-Ausgänge**
- **REQ** – *Request*: Anfrage vom Plug an den Socket („bitte führe X aus“).  
- **RSP** – *Response*: Antwort vom Plug an den Socket, beantwortet eine IND („Indication empfangen“).

### **Daten-Eingänge**
Keine – es werden keine Datenwerte übertragen.

### **Daten-Ausgänge**
Keine – der Adapter ist bewusst datenlos.

### **Adapter**
Der EVENT_HS wird als Adaptertyp verwendet und stellt die Verbindung zwischen zwei FBs her. Er besitzt standardmäßig einen **Plug** (linke Seite) und einen **Socket** (rechte Seite). Die Rollen sind fest definiert: Der Plug agiert als Requester, der Socket als Responder.

## Funktionsweise
Der Adapter definiert zwei spezifische Service-Sequenzen, die die zeitliche Abfolge der Ereignisse festlegen:

1. **request_confirm**  
   - Der Plug sendet ein **REQ**-Ereignis an den Socket.  
   - Der Socket verarbeitet die Anfrage und sendet anschließend ein **CNF**-Ereignis zurück an den Plug.  
   Dieses Muster wird typischerweise für synchrone Aufträge verwendet, bei denen der Aufrufer eine Bestätigung erwartet.

2. **indication_response**  
   - Der Socket sendet ein **IND**-Ereignis an den Plug (z. B. als Folge eines externen Ereignisses).  
   - Der Plug empfängt die Indication und quittiert diese mit einem **RSP**-Ereignis an den Socket.  
   Hierbei handelt es sich um eine asynchrone Benachrichtigung mit anschließender Bestätigung.

Die beiden Sequenzen sind unabhängig voneinander und können zeitlich verschränkt auftreten. Da keine Daten übertragen werden, eignet sich der Adapter für reine Steuer- und Synchronisationsaufgaben.

## Technische Besonderheiten
- **Dataless Design**: Es werden keine Datenvariablen deklariert – ausschließlich Ereignisse werden ausgetauscht. Dies reduziert den Ressourcenbedarf und die Komplexität.
- **Eindeutige Rollenverteilung**: Seit Version 1.1 sind die Ereignis-Eingänge und -Ausgänge vertauscht, sodass der Plug die Requester-Rolle und der Socket die Responder-Rolle einnimmt. Diese Anpassung verbessert die Übersichtlichkeit im Netzwerk-Editor.
- **Compiler-Validierung**: Der ECC-Compiler von 4diac prüft lediglich, ob das referenzierte Ereignis (z. B. `HS.REQ`) auf dem Adapter existiert – nicht jedoch, ob die Richtung an Plug/Socket kohärent ist. Entwickler müssen daher sicherstellen, dass die Verdrahtung und die ECC-Implementierung semantisch korrekt sind.
- **Minimalismus**: Der Adapter bildet exakt das auf Slide 72 des genannten Kurses gezeigte Muster ab, ohne zusätzliche Erweiterungen.

## Zustandsübersicht
Der Adapter selbst besitzt keinen internen Zustandsautomaten. Stattdessen definiert er zwei klar abgegrenzte Service-Sequenzen:
- **Zustand A – Request/Confirm**: Nach dem Senden von `REQ` wartet der Plug auf `CNF`. Diese Sequenz ist synchron.
- **Zustand B – Indication/Response**: Nach dem Empfang von `IND` muss der Plug mit `RSP` antworten. Diese Sequenz ist asynchron.

Die beiden Sequenzen können unabhängig voneinander und auch überlappend ausgeführt werden. Es gibt keine feste Reihenfolge zwischen den Sequenzen.

## Anwendungsszenarien
- **Auftragsbestätigung**: Ein FB sendet eine Anfrage (REQ) an einen anderen FB und wartet auf dessen Bestätigung (CNF), z. B. für das Auslösen einer Aktion mit Rückmeldung.
- **Ereignis-Benachrichtigung**: Ein überwachter Prozess erzeugt ein unerwartetes Ereignis (IND), das an den Plug gemeldet wird. Dieser quittiert den Empfang mit RSP.
- **Steuerung ohne Daten**: Überall dort, wo nur Signale (ohne Daten) ausgetauscht werden müssen, z. B. Trigger, Alarme oder Synchronisationspunkte.
- **Strukturierte Kommunikation**: Statt mehrere einzelne Ereignisverbindungen zu verwenden, bündelt der Adapter die vier Primitive in einer einzigen Schnittstelle – dies erhöht die Lesbarkeit und Wartbarkeit des Systems.

## Vergleich mit ähnlichen Bausteinen
Im Gegensatz zu Adaptern mit Datenübertragung (z. B. `ANY`-basierten Adaptern) überträgt der EVENT_HS ausschließlich Ereignisse. Dadurch entfällt die Notwendigkeit, Datenwerte zu typisieren oder zu serialisieren. Gegenüber der Verwendung von vier separaten Ereignisverbindungen (REQ, CNF, IND, RSP) bietet der Adapter den Vorteil, dass die Zusammengehörigkeit der Primitive explizit gekapselt ist und die Verkabelung im FBNetwork-Editor übersichtlicher wird. Nachteilig ist, dass keine zusätzlichen Informationen (z. B. Fehlercodes oder Nutzdaten) transportiert werden können – dafür sind andere Adapter wie `DATA_HS` oder eigene maßgeschneiderte Typen erforderlich.

## Fazit
Der **EVENT_HS**-Adapter ist eine kompakte und saubere Lösung für reine Ereignissignalisierung nach dem Handshake-Muster. Er eignet sich besonders für einfache Steuer- und Synchronisationsaufgaben, bei denen keine Daten übertragen werden müssen. Durch die klare Rollenverteilung und die intuitive Anordnung im Editor ist er leicht einzusetzen und zu verstehen. Die dokumentierte Einschränkung der Compiler-Validierung sollte jedoch bei der Entwicklung stets beachtet werden, um logische Fehler frühzeitig zu vermeiden. Insgesamt ist der Adapter ein nützliches Werkzeug für die modulare und strukturierte Kommunikation in IEC-61499-Systemen.
# TokenRing

![TokenRing](./TokenRing.svg)

* * * * * * * * * *
## Einleitung
Der TokenRing-Adapter implementiert das Token-Ring-Entwurfsmuster zur gegenseitigen Ausschließung (Mutual Exclusion) in verteilten Systemen. Er ist datenlos und dient ausschließlich zur Übergabe eines Tokens zwischen zwei benachbarten Controllern in einem Ring. Nur der aktuelle Token-Inhaber darf auf die geschützte Ressource zugreifen; nach Abschluss gibt er das Token an seinen Nachbarn weiter.

## Schnittstellenstruktur
Der Adapter besitzt zwei Ereignisse, keine Daten oder Unter-Adapter.

### **Ereignis-Eingänge**
- **RCV**: Ereignis, das vom Socket (Empfänger) an den Plug (Geber) gesendet wird, um den Empfang des Tokens zu bestätigen.

### **Ereignis-Ausgänge**
- **GIVE**: Ereignis, das vom Plug (Geber) an den Socket (Empfänger) gesendet wird, um das Token zu übergeben.

### **Daten-Eingänge**
Keine.

### **Daten-Ausgänge**
Keine.

### **Adapter**
Der TokenRing-Adapter ist selbst ein bidirektionaler Adapter mit zwei Rollen: Plug (rechte Seite, auch als `MTXOUT` bekannt) und Socket (linke Seite, auch als `MTXIN` bekannt). Er enthält keine weiteren Adapter.

## Funktionsweise
Der TokenRing-Adapter implementiert eine einfache Handshake-Sequenz: Der Plug sendet das GIVE-Ereignis an den Socket. Sobald der Socket das Token empfangen hat, antwortet er mit dem RCV-Ereignis an den Plug. Damit ist die Übergabe abgeschlossen. Ein Controller, der in einen Ring eingebunden ist, benötigt zwei Instanzen dieses Adapters: eine als Plug (MTXOUT) für die Verbindung zum nachgeschalteten Nachbarn und eine als Socket (MTXIN) für die Verbindung zum vorgeschalteten Nachbarn. Auf diese Weise wird ein Token im Ring weitergegeben und die gegenseitige Ausschließung sichergestellt.

## Technische Besonderheiten
- Der Adapter ist **datenlos**, d.h. er überträgt keine Nutzdaten, sondern nur Steuersignale.
- Die Ereignisreihenfolge ist durch einen Service-Contract festgelegt: `GIVE` vom Plug zum Socket, gefolgt von `RCV` vom Socket zum Plug.
- Die Rollen sind klar getrennt: Der Plug (MTXOUT) ist der aktive Geber, der Socket (MTXIN) der passive Empfänger.
- Der Adapter ist für eine Ringtopologie konzipiert; jeder Knoten besitzt genau eine Plug- und eine Socket-Instanz.

## Zustandsübersicht
Da es sich um einen reinen Ereignisaustausch ohne interne Zustände handelt, gibt es keinen expliziten Zustandsautomaten. Der Ablauf ist strikt sequenziell und wird durch den Service-Contract `token_pass` definiert:
1. Senden von `GIVE` (Plug → Socket)
2. Empfangen von `RCV` (Socket → Plug)

## Anwendungsszenarien
- **Verteilte Steuerungssysteme**: Mehrere Steuerungen müssen exklusiv auf eine gemeinsame Ressource (z.B. eine Maschinenachse, einen Bus) zugreifen.
- **Ringförmige Netzwerktopologien**: Sicherung der gegenseitigen Ausschließung ohne zentrale Instanz.
- **IEC-61499-basierte Automatisierungssysteme**: Verwendung des Token-Ring-Musters zur Koordination paralleler Funktionsbausteine.

## Vergleich mit ähnlichen Bausteinen
- **Mutual Exclusion (Mutex) Baustein**: Oft als zentrale Semaphore realisiert; der TokenRing-Adapter verteilt die Kontrolle dezentral über eine Ringstruktur.
- **Semaphore-Adapter**: Üblicherweise mit Zähler und Warteschlange; der TokenRing-Adapter arbeitet ohne Zähler und ohne Warteschlange – es gibt immer nur ein Token.
- **Analogie zum Token-Bus**: Der TokenRing-Adapter ist eine logische Ringstruktur, die auf der IEC-61499-Adaptermechanik aufbaut.

## Fazit
Der TokenRing-Adapter ist ein leichtgewichtiges, effektives Mittel zur Implementierung gegenseitiger Ausschließung in verteilten IEC-61499-Systemen. Durch seine einfache Ereignisschnittstelle und die klare Rollentrennung lässt er sich ohne zusätzliche Datenübertragung in Ringtopologien integrieren. Die Verwendung von zwei Adapterinstanzen pro Knoten gewährleistet die korrekte Token-Weitergabe und verhindert gleichzeitigen Ressourcenzugriff.
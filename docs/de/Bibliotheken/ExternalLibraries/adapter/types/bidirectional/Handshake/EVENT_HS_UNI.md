# EVENT_HS_UNI

![EVENT_HS_UNI](./EVENT_HS_UNI.svg)

* * * * * * * * * *

## Einleitung

Der Adapter **EVENT_HS_UNI** gehört zur Familie der Handshake-Adapter nach dem Entwurfsmuster aus IEC 61499 (Modul 6, Valeriy Vyatkin). Er stellt eine stark reduzierte, unidirektionale Variante dar, die ausschließlich ein einzelnes Ereignis **REQ** vom Plug zum Socket überträgt. Es gibt keinerlei Bestätigungs- oder Antwortereignisse (CNF, IND, RSP) – der Adapter implementiert bewusst kein echter Handshake, sondern eine reine „Fire-and-Forget“-Benachrichtigung ohne Rückkanal. Dies macht ihn für Anwendungen geeignet, bei denen eine Bestätigung nicht erforderlich oder nicht gewünscht ist, und wo eine bestmögliche Zustellung ausreicht.

## Schnittstellenstruktur

Der Adapter besitzt nur eine einzige Ereignis-Schnittstelle. Es gibt weder Daten-Ein- noch Daten-Ausgänge, keine Adapter weiter innerhalb des Bausteins und auch keinen zusätzlichen Ereignis-Eingang oder -Ausgang außer dem einen REQ-Ausgang.

### **Ereignis-Eingänge**

Keine

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `REQ` | Event | Anforderung/Benachrichtigung vom Plug zum Socket, keine Antwort erwartet |

### **Daten-Eingänge**

Keine

### **Daten-Ausgänge**

Keine

### **Adapter**

Der Adapter selbst ist ein Plug/Socket-Adapter. Auf der Plug-Seite (linke Schnittstelle) wird das Ereignis `REQ` gesendet, auf der Socket-Seite (rechte Schnittstelle) wird es empfangen. Die Service-Definition dokumentiert diesen Ablauf als Sequenz „notify“.

## Funktionsweise

Der Adapter überträgt ein einzelnes Ereignis `REQ` unidirektional vom Plug zum Socket. Die Übertragung erfolgt ohne Daten und ohne Bestätigung. Der Plug (Absender) löst das Ereignis aus, der Socket (Empfänger) reagiert darauf. Es gibt keine Möglichkeit für den Socket, den Empfang zu quittieren oder die Verarbeitung zu beeinflussen. Der Plug kann nicht feststellen, ob die Benachrichtigung den Socket erreicht hat oder ob sie dort erfolgreich verarbeitet wurde – es handelt sich um eine reine Einweg-Signalisation mit Best-Effort-Charakter.

Der Service-Sequenz-Diagramm zeigt die Transaktion:

- **Plug** sendet `REQ` an **Socket** (event). Es gibt keine Rücktransaktion.

## Technische Besonderheiten

- **Kein Handshake**: Im Gegensatz zu typischen Handshake-Adaptern wie `EVENT_HS` fehlen jegliche Antwortpfade (CNF, IND, RSP). Dadurch ist die Kommunikation extrem schlank und ressourcenschonend, aber auch unzuverlässig im Sinne einer Quittierung.
- **Keine Daten**: Es werden keinerlei Nutzdaten übertragen, nur das Ereignis selbst. Das reduziert die Schnittstellenkomplexität auf ein Minimum.
- **Unidirektionalität**: Die Verbindung erlaubt nur eine Richtung (Plug → Socket). Eine umgekehrte Kommunikation ist nicht vorgesehen.
- **Dokumentationszweck**: Das enthaltene `<Service>`-Element dient lediglich der Dokumentation (optionale Service-Sequenz-Diagramm) und ist für die Funktionalität nicht erforderlich – auch der Standard-Adapter `AE.adp` besitzt kein solches Element.
- **Kompatibilität**: Der Adapter gehört zur Familie der `EVENT_HS`-Adapter und teilt die Rolle Plug/Socket mit diesen. Er ist jedoch nur für den Einsatz als reine Benachrichtigung gedacht und ersetzt nicht einen vollwertigen Handshake.

## Zustandsübersicht

Da der Adapter keinerlei Zustandslogik besitzt, existiert kein expliziter Zustandsautomat. Die einzige Aktion ist das Auslösen des `REQ`-Ereignisses. Es gibt keinen internen Speicher oder Zustand zu verwalten.

## Anwendungsszenarien

- **Simpelste Signalisierung**: Wenn eine Komponente nur eine Information „Es ist etwas passiert“ an eine andere weitergeben muss, ohne Rückantwort oder Daten.
- **Best-Effort-Notify**: Für lose gekoppelte Systeme, bei denen eine Benachrichtigung nicht quittiert werden muss und ein Verlust tolerierbar ist (z. B. Statusmeldungen, die regelmäßig wiederholt werden).
- **Test- und Bildungszwecke**: Als minimales Beispiel für unidirektionale Kommunikation in IEC-61499-Systemen.
- **Reduzierte Variante im Framework**: Innerhalb der `EVENT_HS`-Familie als schlankstes Mitglied – geeignet, wenn die vollwertigen Handshake-Varianten überdimensioniert sind.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Ereignisse | Daten | Richtung | Handshake | Verwendung |
|----------|------------|-------|----------|-----------|------------|
| `EVENT_HS_UNI` (dieser) | Nur `REQ` (Plug → Socket) | Keine | Unidirektional | Nein | Fire-and-Forget-Benachrichtigung |
| `EVENT_HS` | `REQ`/`CNF`, `IND`/`RSP` | Keine | Bidirektional | Ja | Vollständiger Handshake |
| `EVENT_HS_UNI_WSTRING` | Nur `REQ` (Plug → Socket) | WSTRING | Unidirektional | Nein | Fire-and-Forget mit Datennutzlast |
| `EVENT_HS_ACK` | `REQ`/`CNF` | Keine | Bidirektional | Ja | Handshake mit Bestätigung, ohne Daten |
| `EVENT_HS_ACK_WSTRING` | `REQ`/`CNF` | WSTRING | Bidirektional | Ja | Handshake mit Bestätigung und Daten |

Der Unterschied zu `EVENT_HS` besteht vor allem im Wegfall aller Antwort- und Datenpfade. `EVENT_HS_UNI` ist gezielt für einfache, unidirektionale Benachrichtigungen konzipiert.

## Fazit

Der Adapter `EVENT_HS_UNI` ist eine extrem reduzierte Schnittstelle für unidirektionale, darlose Benachrichtigungen. Er eignet sich nur für Szenarien, bei denen eine Bestätigung oder Rückantwort nicht erforderlich ist und die Zustellung als „best effort“ akzeptiert wird. Seine Stärke liegt in der Einfachheit und geringen Ressourcenbelastung; er ist jedoch nicht als Ersatz für einen echten Handshake gedacht. Für Systeme, die eine zuverlässige Kommunikation mit Quittung benötigen, sind die anderen Varianten der `EVENT_HS`-Familie zu wählen.

# A2X_AUI_DEMUX_3

![A2X_AUI_DEMUX_3](./A2X_AUI_DEMUX_3.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein `A2X_AUI_DEMUX_3` ist ein generischer Demultiplexer auf Adapterbasis. Er empfängt über einen `A2X`-Adapter einen Eingangswert und leitet diesen in Abhängigkeit eines über den `AUI`-Adapter anliegenden Index an einen von drei Ausgängen weiter. Der Baustein ist so optimiert, dass ein Adapter-Ausgang nur bei einer tatsächlichen Wertänderung aktualisiert wird und Ereignisse nur bei echten Änderungen ausgelöst werden.

## Schnittstellenstruktur

Der Baustein besitzt keine klassischen Ereignis- oder Datenein-/ausgänge. Die gesamte Kommunikation erfolgt über unidirektionale Adapter.

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

- `CNF` – Bestätigung für das Setzen des Index `K` (`Confirmation of Set Index K`). Dieses Ereignis wird ausgegeben, nachdem ein neuer Index übernommen und der entsprechende Ausgang aktualisiert wurde.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

**Sockets / Eingänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `K`  | `adapter::types::unidirectional::AUI` | Index zur Auswahl des Ausgangs. Gültige Werte: 0, 1, 2. |
| `IN` | `adapter::types::unidirectional::A2X` | Eingangswert, der auf einen der Ausgänge verteilt werden soll. |

**Plugs / Ausgänge**

| Name  | Typ | Beschreibung |
|-------|-----|--------------|
| `OUT1` | `adapter::types::unidirectional::A2X` | Ausgang 1, ausgewählt bei `K = 0`. |
| `OUT2` | `adapter::types::unidirectional::A2X` | Ausgang 2, ausgewählt bei `K = 1`. |
| `OUT3` | `adapter::types::unidirectional::A2X` | Ausgang 3, ausgewählt bei `K = 2`. |

## Funktionsweise

Der Baustein arbeitet als Demultiplexer auf Adapterebene. Ein über den `IN`-Adapter eintreffender Wert wird an denjenigen Ausgang weitergegeben, der durch den aktuellen Wert des `K`-Adapters bestimmt wird.

Der Index `K` wird über den gleichnamigen Socket empfangen. Sobald sich der Index ändert, wird die Zuordnung angepasst. Ein `CNF`-Ereignis bestätigt, dass die Indexänderung übernommen wurde.

Der eigentliche Datenwert wird über den `A2X`-Adapter `IN` empfangen. Nur der aktuell ausgewählte Ausgang erhält den Wert. Die nicht ausgewählten Ausgänge bleiben unverändert.

Eine Besonderheit ist die änderungsbasierte Aktualisierung: Wenn ein neuer Wert empfangen wird, wird geprüft, ob sich der Wert tatsächlich gegenüber dem zuletzt übertragenen Wert geändert hat. Nur in diesem Fall wird der Ausgang aktualisiert und ein Ereignis ausgelöst. Dadurch werden redundante Verarbeitungsschritte und unnötige Ereignisse vermieden.

## Technische Besonderheiten

- **Generischer Funktionsbaustein:** Die konkrete Laufzeitlogik wird über die `GenericClassName` `GEN_A2X_AUI_DEMUX` bereitgestellt. Der vorliegende Baustein ist die konkrete Ausprägung mit drei Ausgängen.
- **Reine Adapter-Schnittstelle:** Es gibt keine direkten Ereignis- oder Datenanschlüsse. Die Kommunikation erfolgt vollständig über die unidirektionalen Adapter `A2X` und `AUI`.
- **Änderungsbasierte Ausgabe:** Der Adapter-Ausgang wird nur bei einer tatsächlichen Wertänderung aktualisiert. Ein Ereignis wird ebenfalls nur bei einer echten Änderung erzeugt.
- **Drei Ausgangskanäle:** `OUT1`, `OUT2` und `OUT3` decken die Indexwerte 0, 1 und 2 ab.
- **Bereitstellungshinweis:** In der vorliegenden Definition ist das Adapter-Backend `GEN_A2X_AUI_DEMUX` als noch zu implementieren gekennzeichnet. Die Schnittstellenbeschreibung ist vollständig; die Laufzeitlogik muss vor dem Einsatz bereitgestellt oder generiert werden.

## Zustandsübersicht

Der generische Baustein besitzt in der XML-Definition keine explizit sichtbare ECC. Logisch lässt sich das Verhalten in folgende Zustände unterteilen:

| Zustand | Auslöser | Verhalten |
|---------|----------|-----------|
| `IDLE` | Keine Änderung | Wartet auf neue Werte oder einen neuen Index. Es findet keine Ausgabeänderung statt. |
| `INDEX_CHANGE` | Neuer Wert am Adapter `K` | Die Auswahl wird auf `OUT1`, `OUT2` oder `OUT3` umgestellt. `CNF` wird ausgelöst. |
| `VALUE_CHANGE` | Neuer Wert am Adapter `IN` | Der Wert wird an den aktuell ausgewählten Ausgang übergeben. Die Aktualisierung erfolgt nur bei tatsächlicher Wertänderung. |

## Anwendungsszenarien

- Verteilung eines zentralen `A2X`-Datenstroms an genau eine von drei möglichen Senken, z. B. an verschiedene Anzeigen, Visualisierungen oder Steuerungsmodule.
- Umschaltung zwischen mehreren Verbrauchern über einen `AUI`-Index, etwa zur Laufzeit- oder Stationsauswahl.
- Entkoppelte Signalverteilung in modularen IEC-61499-Anlagen, bei denen Ereignisse nur bei tatsächlichen Wertänderungen benötigt werden.
- Wiederverwendung als generischer Adapter-Demultiplexer in einer Bibliothek für adapterbasierte Kommunikationsstrukturen.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu einem klassischen Demultiplexer mit getrennten Ereignis- und Dateneingängen besitzt `A2X_AUI_DEMUX_3` keine direkten Datenpins. Die Verbindung erfolgt ausschließlich über unidirektionale Adapter. Dadurch lassen sich typisierte Datenströme direkt an andere Adapter anbinden, ohne zusätzliche Verdrahtung auf Ereignis- und Datenebene.

Der Baustein ist eine `A2X`-Variante des `AX_AUI_DEMUX_3`. Während der Grundaufbau – ein Eingang, drei Ausgänge und ein Index – gleich bleibt, verwendet diese Variante den Adaptertyp `A2X` für den Datentransport. Sie ist damit für Umgebungen geeignet, die durchgängig auf `A2X`-Adapter setzen.

Gegenüber einem Multiplexer, der mehrere Eingänge auf einen Ausgang schaltet, stellt `A2X_AUI_DEMUX_3` die umgekehrte Richtung dar: Ein Eingang wird wahlweise auf einen von drei Ausgängen geschaltet.

## Fazit

`A2X_AUI_DEMUX_3` ist ein kompakter, adapterbasierter Demultiplexer für unidirektionale `A2X`-Datenströme. Seine Stärke liegt in der klaren Trennung zwischen Indexauswahl und Datenweiterleitung sowie in der effizienten, änderungsbasierten Ausgabe. Die generische Definition ermöglicht eine saubere Einbindung in IEC-61499-Systeme, sobald das zugehörige Backend `GEN_A2X_AUI_DEMUX` implementiert ist.

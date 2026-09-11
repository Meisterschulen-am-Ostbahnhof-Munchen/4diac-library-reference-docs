# A2X_AUI_DEMUX_3_UNGATED

![A2X_AUI_DEMUX_3_UNGATED](./A2X_AUI_DEMUX_3_UNGATED.svg)

* * * * * * * * * *

## Einleitung

Der Baustein **A2X_AUI_DEMUX_3_UNGATED** ist ein generischer, adapterbasierter 1-zu-3-Demultiplexer. Er gehört zur A2X/AUI-Familie und leitet einen am Eingangs-Adapter `IN` anliegenden Wert abhängig vom Index `K` an einen der drei Ausgangs-Adapter `OUT1`, `OUT2` oder `OUT3` weiter.

Die Variante **_UNGATED** besitzt **keine Änderungserkennung**. Das bedeutet: Jedes neu berechnete Ergebnis wird bedingungslos an den ausgewählten Ausgang weitergegeben – unabhängig davon, ob sich der Wert gegenüber dem vorherigen geändert hat.

## Schnittstellenstruktur

Der Baustein besitzt keine klassischen Daten-Ein- oder Daten-Ausgänge. Sämtliche Werte und Steuerinformationen werden über die Adapter-Sockets und Adapter-Plugs übertragen.

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `CNF` | Event | Bestätigung, dass der Index `K` gesetzt beziehungsweise verarbeitet wurde. |

### **Daten-Eingänge**

Keine. Alle Eingangsdaten werden über die Adapter-Sockets `IN` und `K` transportiert.

### **Daten-Ausgänge**

Keine. Alle Ausgangsdaten werden über die Adapter-Plugs `OUT1`, `OUT2` und `OUT3` transportiert.

### **Adapter**

| Name | Richtung | Typ | Kommentar |
|------|----------|-----|-----------|
| `K` | Socket (Eingang) | `adapter::types::unidirectional::AUI` | Index zur Auswahl des Ausgangs |
| `IN` | Socket (Eingang) | `adapter::types::unidirectional::A2X` | Eingangswert, der demultiplext wird |
| `OUT1` | Plug (Ausgang) | `adapter::types::unidirectional::A2X` | Ausgangswert 1, ausgewählt bei K = 0 |
| `OUT2` | Plug (Ausgang) | `adapter::types::unidirectional::A2X` | Ausgangswert 2, ausgewählt bei K = 1 |
| `OUT3` | Plug (Ausgang) | `adapter::types::unidirectional::A2X` | Ausgangswert 3, ausgewählt bei K = 2 |

## Funktionsweise

Der Baustein arbeitet als Demultiplexer auf Adapterebene:

1. Über den Socket `K` wird der gewünschte Ausgangskanal ausgewählt.
2. Über den Socket `IN` wird der weiterzuleitende Wert empfangen.
3. Der Wert wird an den ausgewählten Ausgang weitergegeben:
   - K = 0 → `OUT1`
   - K = 1 → `OUT2`
   - K = 2 → `OUT3`
4. Das Ereignis `CNF` wird ausgelöst, nachdem der Index `K` verarbeitet wurde.

Da es sich um die **_UNGATED**-Variante handelt, wird keine Prüfung durchgeführt, ob sich der Eingangswert geändert hat. Jeder ankommende Wert wird unabhängig von einer Wertänderung weitergeleitet. Dadurch eignet sich der Baustein besonders für Verbraucher, die eine periodische Kadenz benötigen – etwa für Ableitungs- oder Frequenzberechnungen.

## Technische Besonderheiten

- Der Baustein ist als generische Klasse gekennzeichnet:  
  `eclipse4diac::core::GenericClassName = 'GEN_A2X_AUI_DEMUX'`
- Er liegt im Paket `adapter::selection::unidirectional`.
- Der Baustein besitzt keine klassischen Ereignis-Eingänge. Die Ansteuerung erfolgt über die verbundenen Adapter-Sockets.
- Alle Adapter sind unidirektional ausgelegt.
- Es gibt keine Änderungserkennung und keine Filterung des Eingangswerts.
- Laut Versionsinformation ist das eigentliche Adapter-Backend `GEN_A2X_AUI_DEMUX` in dieser Version noch zu implementieren. Der Baustein ist daher als generische Vorlage beziehungsweise als Entwurf zu betrachten.

## Zustandsübersicht

In der übergebenen XML ist keine explizite ECC-Zustandsmaschine hinterlegt. Das erwartete konzeptionelle Verhalten lässt sich wie folgt beschreiben:

| Zustand | Bedeutung |
|---------|-----------|
| `IDLE` | Warten auf gültigen Index `K` und/oder Eingangswert `IN` |
| `SELECT` | Auswahl des Zielausgangs anhand von `K` |
| `FORWARD` | Bedingungslose Weiterleitung von `IN` an den gewählten Ausgang |
| `CONFIRM` | Auslösen des Ereignisses `CNF` nach Verarbeitung von `K` |

## Anwendungsszenarien

- **Periodische Frequenz- oder Ableitungsberechnung:**  
  Verbraucher benötigen in jedem Zyklus einen neuen Wert, auch wenn der Zahlenwert unverändert bleibt.

- **Signalverteilung auf mehrere Auswertepfade:**  
  Ein Eingangswert wird abhängig vom Index an einen von drei Verarbeitungspfaden weitergeleitet.

- **Zyklische Messwertübertragung:**  
  Jeder berechnete Messwert wird ohne vorherige Änderungsprüfung an den ausgewählten Ausgang gesendet.

- **Generischer Aufbau:**  
  Durch die Verwendung von Adaptern kann der Baustein mit unterschiedlichen A2X- und AUI-Datentypen verbunden werden.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Änderungserkennung | Verhalten |
|----------|--------------------|-----------|
| `A2X_AUI_DEMUX_3` | Ja | Gibt Werte nur bei Änderung weiter |
| `A2X_AUI_DEMUX_3_UNGATED` | Nein | Gibt jeden Wert bedingungslos weiter |

Gegenüber Varianten mit Änderungserkennung verzichtet der _UNGATED_-Baustein bewusst auf Logik zur Unterdrückung unveränderter Werte. Dadurch wird sichergestellt, dass Verbraucher mit einer festen zeitlichen Kadenz immer ein aktuelles Ergebnis erhalten.

## Fazit

`A2X_AUI_DEMUX_3_UNGATED` ist ein generischer, adapterbasierter 1-zu-3-Demultiplexer ohne Änderungserkennung. Er leitet jeden ankommenden Wert unabhängig von dessen Änderung an den durch `K` gewählten Ausgang weiter. Damit eignet er sich besonders für periodische und taktgebundene Anwendungen. Da das Adapter-Backend `GEN_A2X_AUI_DEMUX` laut Versionsinformation noch implementiert werden muss, sollte der Baustein aktuell nur als Entwurf betrachtet werden.

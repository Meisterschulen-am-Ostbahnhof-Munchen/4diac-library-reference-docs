# A2X_AUI_DEMUX_2_UNGATED

![A2X_AUI_DEMUX_2_UNGATED](./A2X_AUI_DEMUX_2_UNGATED.svg)

* * * * * * * * * *

## Einleitung

Der Baustein **A2X_AUI_DEMUX_2_UNGATED** ist ein generischer, adapterbasierter 1-aus-2-Demultiplexer. Er leitet einen über den Adapter `IN` ankommenden A2X-Wert an einen der beiden Ausgänge `OUT1` oder `OUT2` weiter. Die Auswahl des Ausgangs erfolgt über den Index-Adapter `K`.

Die Erweiterung **UNGATED** bedeutet, dass keine Änderungserkennung stattfindet. Jedes neu berechnete Ergebnis wird bedingungslos und ohne Filterung an den ausgewählten Ausgang weitergereicht – auch dann, wenn sich der Wert gegenüber dem vorherigen Zyklus nicht geändert hat.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine separaten Ereignis-Eingänge vorhanden.  
Die auslösenden Ereignisse werden über die Adapter-Schnittstellen transportiert.

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `CNF` | Event | Bestätigung, dass der Index `K` gesetzt wurde |

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

| Adapter | Typ | Richtung | Kommentar |
|---------|-----|----------|-----------|
| `OUT1` | `adapter::types::unidirectional::A2X` | Plug | Ausgangswert 1, ausgewählt wenn `K = 0` |
| `OUT2` | `adapter::types::unidirectional::A2X` | Plug | Ausgangswert 2, ausgewählt wenn `K = 1` |
| `K` | `adapter::types::unidirectional::AUI` | Socket | Auswahlindex |
| `IN` | `adapter::types::unidirectional::A2X` | Socket | Eingangswert zum Demultiplexen |

## Funktionsweise

Der Baustein arbeitet als Demultiplexer:

- Der aktuelle A2X-Wert liegt am Socket `IN` an.
- Der Socket `K` liefert den Auswahlindex.
- Bei `K = 0` wird der Wert an `OUT1` weitergeleitet.
- Bei `K = 1` wird der Wert an `OUT2` weitergeleitet.

Der Zusatz **UNGATED** ist die entscheidende Eigenschaft: Es findet keine Erkennung statt, ob sich der Wert geändert hat. Jeder neue Berechnungsschritt beziehungsweise jedes neue Ergebnis wird unmittelbar und bedingungslos an den ausgewählten Ausgang weitergegeben.

Über das Ereignis `CNF` wird bestätigt, dass der an `K` anliegende Index übernommen wurde.

## Technische Besonderheiten

- Der Baustein ist als generischer Funktionsbaustein angelegt.  
  Über das Attribut `eclipse4diac::core::GenericClassName = 'GEN_A2X_AUI_DEMUX'` wird die generische Implementierung referenziert.
- Die Adapter gehören zum Paket `adapter::selection::unidirectional`.
- Es gibt keine direkten Daten-Eingänge oder Daten-Ausgänge. Die Daten werden vollständig über die unidirektionalen Adapter übertragen.
- `UNGATED` bedeutet: keine Unterdrückung unveränderter Werte, keine Flankenerkennung, keine Änderungsfilterung.
- Die Versionsinformation weist darauf hin, dass das Adapter-Backend `GEN_A2X_AUI_DEMUX` in dieser Version noch zu implementieren ist.

## Zustandsübersicht

Der Baustein besitzt keine explizite Zustandsmaschine beziehungsweise keinen eigenen ECC. Das Verhalten ist reaktiv und wird über die Adapter angestoßen. Logisch lassen sich folgende Phasen unterscheiden:

| Phase | Beschreibung |
|-------|--------------|
| Indexübernahme | Der Socket `K` liefert den Auswahlindex `0` oder `1`. |
| Wertweiterleitung | Der aktuelle `IN`-Wert wird an `OUT1` oder `OUT2` übergeben. |
| Quittung | Das Ereignis `CNF` signalisiert die Übernahme des Indexes. |

Da keine Änderungserkennung erfolgt, wird auch ein unveränderter Wert in jedem Zyklus erneut weitergeleitet.

## Anwendungsszenarien

- **Periodische Ableitungsberechnung:** Ein Messwert wird zyklisch an einen Differenzierer weitergegeben. Auch unveränderte Werte müssen ankommen, damit die zeitliche Kadenz erhalten bleibt.
- **Frequenzberechnung:** Gleiche aufeinanderfolgende Werte dürfen nicht ausgeblendet werden, weil die Frequenzbestimmung auf der Zählung aller Abtastwerte beruht.
- **Signalverteilung:** Ein A2X-Wert wird je nach Betriebsmodus an einen von zwei Verbrauchern geroutet, ohne dass gleiche Werte unterdrückt werden.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Eigenschaft |
|----------|-------------|
| `A2X_AUI_DEMUX_2` | Gleiche Struktur, jedoch mit Änderungserkennung: Es werden nur Werte weitergegeben, die sich gegenüber dem vorherigen Wert geändert haben. |
| `A2X_AUI_DEMUX_2_UNGATED` | Ungefilterte Weitergabe: Jedes neu berechnete Ergebnis wird unabhängig von einer Wertänderung weitergeleitet. |
| `AX_AUI_DEMUX_2_UNGATED` | Frühere Variante mit AX-Adapter statt A2X; die Demultiplexerlogik ist grundsätzlich vergleichbar. |

## Fazit

`A2X_AUI_DEMUX_2_UNGATED` ist ein generischer Demultiplexer für adapterbasierte A2X-Werte. Seine Stärke liegt in der kadenztreuen Weitergabe jedes Ergebnisses ohne Änderungsfilterung. Damit ist er besonders geeignet für Verbraucher, die eine periodische Aktualisierung benötigen, wie etwa Ableitungs- oder Frequenzberechnungen. Vor dem Einsatz sollte jedoch der Implementierungsstand des Backends `GEN_A2X_AUI_DEMUX` geprüft werden.

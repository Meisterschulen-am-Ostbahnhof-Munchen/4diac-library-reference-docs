# BargraphSplitFS_AR

![BargraphSplitFS_AR](./BargraphSplitFS_AR.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **BargraphSplitFS_AR** ist ein Adapter-Wrapper um den Baustein **BargraphSplitFS**. Er erweitert dessen Funktionalität, indem er die Eingabe eines vorzeichenbehafteten physikalischen Werts nicht über ein herkömmliches `REQ`/`rValue`-Paar, sondern über einen AR-Adapter-Socket (unidirektional) entgegennimmt. Dadurch lässt sich der Baustein nahtlos in eine adapterbasierte Kommunikationsstruktur einbinden, wie sie beispielsweise in ISO-11783-Netzwerken (ISOBUS) üblich ist. Die Ausgänge für Status und Ergebnisse sowie die Überlauf-Meldungen werden unverändert durchgereicht.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ | Mit Variablen | Kommentar |
|----------|-----|---------------|-----------|
| `INIT`   | `EInit` | `stObj` | Service-Initialisierung; übernimmt die Objekt-Pool-Eigenschaften für das Split-Bargraph. |

### **Ereignis-Ausgänge**

| Ereignis | Typ | Mit Variablen | Kommentar |
|----------|-----|---------------|-----------|
| `INITO`  | `EInit` | – | Bestätigung der Initialisierung. |
| `CNF`    | `Event` | `STATUSRight`, `s16resultRight`, `STATUSLeft`, `s16resultLeft` | Bestätigung des angeforderten Dienstes; enthält Status- und Ergebniswerte beider Bargraph-Seiten. |

### **Daten-Eingänge**

| Variable | Typ | Kommentar |
|----------|-----|-----------|
| `stObj`  | `isobus::utils::bargraph::BargraphSplit_S` | Objekt-Pool-Eigenschaften (Referenzen auf linken/rechten Bargraph, gemeinsame Magnituden-Grenzen), werden bei `INIT` übernommen. |

### **Daten-Ausgänge**

| Variable | Typ | Kommentar |
|----------|-----|-----------|
| `STATUSRight` | `STRING` | Dienststatus der rechten Seite – Durchreichung vom internen BargraphSplitFS. |
| `s16resultRight` | `INT` | Ergebniswert (Rückgabewert) der rechten Seite – Durchreichung. |
| `STATUSLeft`  | `STRING` | Dienststatus der linken Seite – Durchreichung. |
| `s16resultLeft` | `INT` | Ergebniswert der linken Seite – Durchreichung. |

### **Adapter**

| Adapter | Richtung | Typ | Kommentar |
|---------|----------|-----|-----------|
| `rPhys` | Socket (Eingang) | `adapter::types::unidirectional::AR` | Vorzeichenbehafteter physikalischer Eingangswert. |
| `xOverRight` | Plug (Ausgang) | `adapter::types::unidirectional::AX` | Signalisiert, dass der Wert die rechte Magnituden-Grenze überschritten hat (geklemmt). |
| `xOverLeft`  | Plug (Ausgang) | `adapter::types::unidirectional::AX` | Signalisiert, dass der negative Wert die linke Magnituden-Grenze überschritten hat (geklemmt). |

## Funktionsweise

Der Baustein **BargraphSplitFS_AR** enthält intern eine Instanz des Funktionsblocks `BargraphSplitFS` (Name: `Inner`). Beim `INIT`-Ereignis wird das übergebene `stObj` direkt an den internen Baustein weitergeleitet und dort initialisiert. Der eingehende Wert am AR-Adapter `rPhys` wird über dessen Datenausgang `D1` auf das Dateneingang `rValue` des internen Bausteins gespeist; das Ereignis `E1` des Adapters löst dabei das `REQ`-Ereignis des internen Bausteins aus. Dadurch wird die Verarbeitung ohne zusätzliche manuelle Triggerung gestartet.

Die Ergebnisse und Statusmeldungen des internen Bausteins werden unverändert auf die entsprechenden Ausgänge des Adapter-Wrappers durchgeschaltet. Die Überlauf-Signale (`xOverRight`, `xOverLeft`) werden als AX-Adapter-Plugs zur Verfügung gestellt und vom internen Baustein übernommen. Das `CNF`-Ereignis des internen Bausteins wird sowohl auf den `CNF`-Ausgang des Wrappers als auch auf die Ereigniseingänge der beiden AX-Plugs gelegt, sodass eine Weiterverarbeitung synchron erfolgen kann.

## Technische Besonderheiten

- **Adapterbasierte Eingabe:** Der Wert wird nicht über ein separates `REQ`-Ereignis und eine Datenvariable, sondern über einen AR-Adapter empfangen. Dadurch ist der Baustein besonders für modulare, adaptergesteuerte Datenflussarchitekturen geeignet.
- **Reine Durchreichung:** Der Wrapper fügt keine zusätzliche Logik hinzu, sondern leitet alle Signale transparent an die interne `BargraphSplitFS`-Instanz weiter.
- **Zwei getrennte Überlauf-Ausgänge:** Die Überlauf-Informationen für rechte und linke Seite werden separat als AX-Adapter-Plugs exponiert.
- **Direkte Kopplung von Ereignis und Daten:** Das Auslösen der Verarbeitung erfolgt automatisch durch das Eintreffen eines neuen Werts am AR-Adapter – es ist kein externes Triggern erforderlich.

## Zustandsübersicht

Der Baustein selbst besitzt keine eigene Zustandsmaschine. Er delegiert die gesamte Zustandslogik an den internen `BargraphSplitFS`. Der interne Baustein durchläuft dabei typischerweise Zustände wie:

- **IDLE**: Warten auf ein `REQ`-Ereignis.
- **BUSY**: Verarbeitung läuft.
- **DONE**: Verarbeitung abgeschlossen, `CNF` wird ausgelöst.

Da der Wrapper diese Zustände nicht verändert, ist eine gesonderte Zustandsbeschreibung nicht erforderlich.

## Anwendungsszenarien

- **ISOBUS-Anzeigen:** Einsatz in Traktor- oder Anbaugeräten, bei denen ein Bargraph mit zwei separaten Anzeigebereichen (links/rechts) über einen Adapter mit einem zentralen Steuergerät verbunden ist.
- **Adapterbasierte Wertübergabe:** Überall dort, wo Werte nicht über klassische Datenports, sondern über AR/AX-Adapter ausgetauscht werden sollen – z. B. in einer modularen Objekt-Pool-Architektur nach ISO 11783-6.
- **Erweiterung bestehender Systeme:** Der Baustein erlaubt es, ein bestehendes `BargraphSplitFS`-Design ohne Änderung des Kernalgorithmus in ein adapterorientiertes Umfeld zu integrieren.

## Vergleich mit ähnlichen Bausteinen

Gegenüber dem Basisbaustein `BargraphSplitFS` bietet der Wrapper eine **adapterbasierte Eingabe** statt einer direkten `REQ`/`rValue`-Schnittstelle. Dies vereinfacht die Einbindung in Adapter-Topologien und erhöht die Wiederverwendbarkeit in modularen Systemen. Ähnliche Wrapper existieren z. B. als `PositionMarkerFSA` für Positionsmarker; `BargraphSplitFS_AR` folgt dem gleichen Entwurfsmuster und stellt die Überlauf-Signale analog über AX-Plugs bereit. Im Gegensatz zu einer vollständig eigenständigen Neuimplementierung bleibt die bewährte Funktionalität des Kernbausteins unverändert erhalten.

## Fazit

**BargraphSplitFS_AR** ist ein sauber implementierter Adapter-Wrapper, der die Funktionalität von `BargraphSplitFS` um eine AR-basierte Eingangsschnittstelle ergänzt. Er eignet sich hervorragend für den Einsatz in ISOBUS-Anwendungen und adapterorientierten Architekturen. Durch die transparente Durchreichung aller relevanten Signale bleiben Verhalten und Zuverlässigkeit des ursprünglichen Bausteins vollständig erhalten, während die Integration in moderne Kommunikationsmuster erleichtert wird.

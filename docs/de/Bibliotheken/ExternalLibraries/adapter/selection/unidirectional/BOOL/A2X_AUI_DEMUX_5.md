# A2X_AUI_DEMUX_5

![A2X_AUI_DEMUX_5](./A2X_AUI_DEMUX_5.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `A2X_AUI_DEMUX_5` ist ein Demultiplexer mit fünf Ausgängen. Er leitet den am Adapter-Socket `IN` anliegenden Wert an einen der fünf Adapter-Plugs `OUT1` bis `OUT5` weiter. Die Auswahl des aktiven Ausgangs erfolgt über den Indexwert am Adapter-Socket `K`. Es handelt sich um eine A2X-Variante von `AX_AUI_DEMUX_5` mit fester Ausgangsanzahl von 5. Der Baustein ist als generischer Funktionsblock auf Basis des Generik-Gerüsts `GEN_A2X_AUI_DEMUX` modelliert.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Baustein besitzt keine Ereignis-Eingänge.

### **Ereignis-Ausgänge**

| Name | Typ   | Kommentar |
|------|-------|-----------|
| `CNF` | Event | Bestätigung (Confirmation) für das Setzen des Indexes `K`. |

### **Daten-Eingänge**

Der Baustein besitzt keine Daten-Eingänge.

### **Daten-Ausgänge**

Der Baustein besitzt keine Daten-Ausgänge.

### **Adapter**

Der Baustein verwendet ausschließlich Adapter-Schnittstellen zur Übertragung von Werten und Steuerinformationen.

**Plugs (Ausgänge):**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `OUT1` | `adapter::types::unidirectional::A2X` | Ausgangswert 1 (aktiv bei K = 0) |
| `OUT2` | `adapter::types::unidirectional::A2X` | Ausgangswert 2 (aktiv bei K = 1) |
| `OUT3` | `adapter::types::unidirectional::A2X` | Ausgangswert 3 (aktiv bei K = 2) |
| `OUT4` | `adapter::types::unidirectional::A2X` | Ausgangswert 4 (aktiv bei K = 3) |
| `OUT5` | `adapter::types::unidirectional::A2X` | Ausgangswert 5 (aktiv bei K = 4) |

**Sockets (Eingänge):**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `K` | `adapter::types::unidirectional::AUI` | Index zur Auswahl des Ausgangs |
| `IN` | `adapter::types::unidirectional::A2X` | Eingangswert, der demultiplext wird |

## Funktionsweise

Der Demultiplexer leitet den über `IN` empfangenen Wert an den durch `K` bestimmten Ausgang weiter. Dabei gilt:

- `K = 0` → Ausgabe auf `OUT1`
- `K = 1` → Ausgabe auf `OUT2`
- `K = 2` → Ausgabe auf `OUT3`
- `K = 3` → Ausgabe auf `OUT4`
- `K = 4` → Ausgabe auf `OUT5`

Der Baustein arbeitet rein wertänderungsbasiert. Ein Adapter-Ausgang wird nur dann aktualisiert, wenn sich der Wert tatsächlich ändert. Das Ereignis `CNF` wird nur ausgesendet, wenn bei einer Änderung des Indexes `K` oder des Eingangswertes `IN` eine tatsächliche Wertänderung an einem Ausgang stattgefunden hat.

## Technische Besonderheiten

- Der Baustein ist eine **A2X-Variante** von `AX_AUI_DEMUX_5` und nutzt den Adaptertyp `A2X` für Ein- und Ausgänge sowie `AUI` für den Index.
- Er ist als **generischer Funktionsblock** angelegt. Die generische Klasse lautet `GEN_A2X_AUI_DEMUX`. Die vorliegende Instanz `A2X_AUI_DEMUX_5` hat fest fünf Ausgänge.
- Der Baustein enthält keine klassischen Daten- oder Ereignis-Ein-/Ausgänge; die gesamte Kommunikation erfolgt über Adapter.
- Das Ausgabe-Verhalten ist ereignisgesteuert, jedoch wird `CNF` nur bei tatsächlicher Wertänderung ausgelöst.
- Die Paketbezeichnung `adapter::selection::unidirectional` ordnet den Baustein in eine Bibliothek für unidirektionale Selektions-Adapter ein.

## Zustandsübersicht

Eine explizite Zustandsmaschine ist für diesen Funktionsblock nicht hinterlegt. Das Verhalten wird durch den generischen Backend-Baustein `GEN_A2X_AUI_DEMUX` realisiert. Konzeptionell lässt sich der Ablauf in zwei Phasen beschreiben:

1. **Bereitschaft**: Der Baustein wartet auf einen neuen Wert an `IN` oder einen neuen Indexwert an `K`.
2. **Weiterleitung**: Sobald sich der Index oder der Eingangswert ändert, wird der entsprechende Ausgang mit dem Wert belegt und nach erfolgreicher Aktualisierung das Ereignis `CNF` ausgesendet.

Da keine expliziten Zustände definiert sind, beschreibt dieses Modell das erwartete Kommunikationsverhalten.

## Anwendungsszenarien

- **Auswahl eines von mehreren Zielen**: Ein Messwert oder Datentelegramm wird über den Eingang `IN` empfangen und je nach Index an einen von fünf nachgeschalteten Bausteinen weitergeleitet.
- **Routing in modularen Automatisierungssystemen**: In einer Steuerungsarchitektur können Datenkanäle abhängig von einem Index auf verschiedene Verbraucher oder Senken geschaltet werden.
- **Test- und Simulationsaufbauten**: Der Baustein kann verwendet werden, um Werte gezielt an verschiedene Testpfade zu verteilen, ohne die Verdrahtung zu ändern.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu einem klassischen Multiplexer (mehrere Eingänge, ein Ausgang) führt dieser Baustein eine Demultiplex-Funktion aus. Während Varianten wie `AX_AUI_DEMUX_5` möglicherweise herkömmliche Daten- und Ereignisschnittstellen verwenden, setzt `A2X_AUI_DEMUX_5` konsequent auf die unidirektionalen Adaptertypen `A2X` und `AUI`. Dadurch wird die Integration in adapterbasierte Kommunikationsstrukturen erleichtert. Durch die wertänderungsbasierte Aktualisierung und die daraus folgende bedingte `CNF`-Ausgabe unterscheidet er sich von Bausteinen, die bei jedem Empfang eines Ereignisses einen Ausgang unabhängig von einer Wertänderung aktualisieren.

## Fazit

`A2X_AUI_DEMUX_5` ist ein spezialisierter Demultiplexer mit fünf Ausgängen für unidirektionale Adapterverbindungen. Er eignet sich für den wertabhängigen Datentransfer in modularen Automatisierungs- und Kommunikationsarchitekturen. Die Beschränkung auf Adapterschnittstellen und die wertänderungsbasierte Informationsweitergabe ermöglichen eine präzise und effiziente Weiterleitung von Daten, ohne unnötige Ereignisse zu erzeugen. Der Baustein stellt damit eine nützliche Komponente für adapterbasierte Selektions- und Verteilaufgaben dar.

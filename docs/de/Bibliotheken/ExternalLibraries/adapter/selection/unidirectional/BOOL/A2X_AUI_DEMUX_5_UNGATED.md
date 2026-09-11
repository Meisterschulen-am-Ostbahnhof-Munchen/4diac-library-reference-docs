# A2X_AUI_DEMUX_5_UNGATED

![A2X_AUI_DEMUX_5_UNGATED](./A2X_AUI_DEMUX_5_UNGATED.svg)

* * * * * * * * * *

## Einleitung

Der Baustein **A2X_AUI_DEMUX_5_UNGATED** ist ein unidirektionaler Demultiplexer mit fünf Ausgängen. Er empfängt über den Adapter-Socket `IN` einen Wert und über den Adapter-Socket `K` einen Index. Abhängig vom Index wird der empfangene Wert an einen der fünf Ausgangs-Adapter `OUT1` bis `OUT5` weitergeleitet.

Die Variante **UNGATED** verzichtet auf eine Änderungserkennung. Das bedeutet: Jedes neu ankommende Ergebnis wird bedingungslos an den ausgewählten Ausgang weitergegeben – auch dann, wenn sich der Wert gegenüber der vorherigen Übertragung nicht geändert hat.

* * * * * * * * * *

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine eigenständigen Ereignis-Eingänge. Die verarbeitungsrelevanten Ereignisse werden über die Adapter-Sockets transportiert.

### **Ereignis-Ausgänge**

| Name | Typ   | Beschreibung |
|------|-------|--------------|
| CNF  | Event | Bestätigung der Verarbeitung von Index K bzw. der erfolgten Ausgabe. |

### **Daten-Eingänge**

Keine direkten Daten-Eingänge. Die Daten werden über die Adapter-Sockets `IN` und `K` geführt.

### **Daten-Ausgänge**

Keine direkten Daten-Ausgänge. Die Ausgangsdaten werden über die Adapter-Plugs `OUT1` bis `OUT5` bereitgestellt.

### **Adapter**

| Adapter | Richtung | Typ                                  | Beschreibung |
|---------|----------|--------------------------------------|--------------|
| `K`     | Socket   | `adapter::types::unidirectional::AUI` | Index zur Auswahl eines Ausgangs |
| `IN`    | Socket   | `adapter::types::unidirectional::A2X` | Eingangswert, der demultiplexiert werden soll |
| `OUT1`  | Plug     | `adapter::types::unidirectional::A2X` | Ausgang 1, aktiv bei K = 0 |
| `OUT2`  | Plug     | `adapter::types::unidirectional::A2X` | Ausgang 2, aktiv bei K = 1 |
| `OUT3`  | Plug     | `adapter::types::unidirectional::A2X` | Ausgang 3, aktiv bei K = 2 |
| `OUT4`  | Plug     | `adapter::types::unidirectional::A2X` | Ausgang 4, aktiv bei K = 3 |
| `OUT5`  | Plug     | `adapter::types::unidirectional::A2X` | Ausgang 5, aktiv bei K = 4 |

* * * * * * * * * *

## Funktionsweise

Der Baustein arbeitet als 1-aus-5-Demultiplexer auf Adapterbasis.

1. Über den Socket `K` wird der gewünschte Ausgangskanal als Index empfangen.
2. Über den Socket `IN` trifft der zu verteilende Wert ein.
3. Der Wert wird an den durch `K` ausgewählten Ausgangs-Adapter `OUT1` bis `OUT5` weitergeleitet.
4. Nach der Weiterleitung wird das Ereignis `CNF` ausgegeben.

Die Zuordnung lautet:

- K = 0 → `OUT1`
- K = 1 → `OUT2`
- K = 2 → `OUT3`
- K = 3 → `OUT4`
- K = 4 → `OUT5`

Durch die **UNGATED**-Eigenschaft gibt es keine Filterung auf Wertänderungen. Ein konstanter Wert wird also bei jedem Ereignis erneut an den gewählten Ausgang übertragen.

* * * * * * * * * *

## Technische Besonderheiten

- **Keine Änderungserkennung:** Der Baustein leitet jedes neu berechnete Ergebnis ohne Bedingung weiter.
- **Adapterbasierte Ein-/Ausgabe:** Die Datenübertragung erfolgt über unidirektionale Adapter vom Typ `A2X` und `AUI`.
- **Generischer Baustein:** Der Baustein ist als generischer FB deklariert und verwendet die generische Klasse `GEN_A2X_AUI_DEMUX`.
- **Kein ECC:** Es ist kein internes Zustandsdiagramm beziehungsweise keine Ablaufsteuerung (ECC) vorhanden.
- **Hinweis:** Laut Versionsinformation ist das zugehörige Adapter-Backend `GEN_A2X_AUI_DEMUX` noch zu implementieren.

* * * * * * * * * *

## Zustandsübersicht

Der Baustein besitzt keine explizite Zustandsmaschine. Die logische Auswahl wird über den Index `K` bestimmt.

| K  | Aktiver Ausgang |
|----|-----------------|
| 0  | `OUT1` |
| 1  | `OUT2` |
| 2  | `OUT3` |
| 3  | `OUT4` |
| 4  | `OUT5` |
| Sonstige Werte | Nicht definiert / kein Ausgang spezifiziert |

* * * * * * * * * *

## Anwendungsszenarien

- **Ableitungs- und Frequenzberechnungen:** Verbraucher, die eine periodische Kadenz benötigen, erhalten auch bei unveränderten Werten weiterhin ein Ereignis.
- **Kanalwahl:** Ein einzelner Messwert kann abhängig von einer Steuergröße gezielt an verschiedene Ausgänge verteilt werden.
- **Zyklische Datenverteilung:** Der FB eignet sich für Systeme, bei denen jeder Takt eine aktualisierte Ausgabe erwartet wird, unabhängig davon, ob sich der Datenwert geändert hat.
- **Test- und Simulationsumgebungen:** Zur gezielten Umschaltung zwischen mehreren Datenempfängern über einen zentralen Index.

* * * * * * * * * *

## Vergleich mit ähnlichen Bausteinen

| Baustein | Änderungserkennung | Ausgabeverhalten |
|----------|--------------------|------------------|
| `A2X_AUI_DEMUX_5` | Ja | Werte werden nur bei Änderung weitergegeben |
| `A2X_AUI_DEMUX_5_UNGATED` | Nein | Jeder Wert wird ungefiltert weitergegeben |

Gegenüber einem klassischen Demultiplexer mit getrennten Daten- und Ereignisports kapselt dieser Baustein die Datenpfade in Adaptern. Dadurch lassen sich Verbindungen modularer und typischerweise einfacher in der Applikation einsetzen.

* * * * * * * * * *

## Fazit

Der Baustein **A2X_AUI_DEMUX_5_UNGATED** ist ein adapterbasierter 5-fach-Demultiplexer ohne Änderungserkennung. Er eignet sich besonders für Verbraucher, die auch bei unveränderten Werten eine feste Versorgung mit Ereignissen und Daten benötigen. Die generische Ausführung macht ihn flexibel, solange das Backend `GEN_A2X_AUI_DEMUX` in der Zielumgebung verfügbar ist.

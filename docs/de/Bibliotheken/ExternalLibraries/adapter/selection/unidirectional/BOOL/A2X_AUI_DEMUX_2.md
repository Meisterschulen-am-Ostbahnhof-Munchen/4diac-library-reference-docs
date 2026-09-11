# A2X_AUI_DEMUX_2

![A2X_AUI_DEMUX_2](./A2X_AUI_DEMUX_2.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **A2X_AUI_DEMUX_2** ist ein generischer Demultiplexer, der einen über einen Adapter anliegenden Wert in Abhängigkeit eines Indexes an einen von zwei Ausgängen weiterleitet. Die Schnittstellen sind vollständig als unidirektionale Adapter realisiert. Der Baustein aktualisiert einen Adapter-Ausgang nur bei einer tatsächlichen Wertänderung; das zugehörige Ereignis wird ebenfalls nur dann ausgelöst.

## Schnittstellenstruktur

Der Baustein besitzt keine klassischen Datenports. Der Datenaustausch erfolgt über Adapter-Sockets und Adapter-Plugs. Es ist ein Ereignisausgang vorhanden.

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `CNF` | Event | Bestätigung der Übernahme des Index `K`. |

### **Daten-Eingänge**

Keine. Der Datentransport erfolgt über Adapter.

### **Daten-Ausgänge**

Keine. Der Datentransport erfolgt über Adapter.

### **Adapter**

| Richtung | Name | Typ | Beschreibung |
|---|---|---|---|
| Socket | `IN` | `adapter::types::unidirectional::A2X` | Eingangswert, der demultiplext wird. |
| Socket | `K` | `adapter::types::unidirectional::AUI` | Auswahlindex: `K = 0` wählt `OUT1`, `K = 1` wählt `OUT2`. |
| Plug | `OUT1` | `adapter::types::unidirectional::A2X` | Ausgang 1, aktiv bei `K = 0`. |
| Plug | `OUT2` | `adapter::types::unidirectional::A2X` | Ausgang 2, aktiv bei `K = 1`. |

## Funktionsweise

Der Demultiplexer arbeitet wie ein umschaltbarer Verteiler:

- Der am Socket `IN` anliegende Wert wird unverändert an den ausgewählten Adapter-Ausgang weitergeleitet.
- Bei `K = 0` wird `OUT1` aktualisiert.
- Bei `K = 1` wird `OUT2` aktualisiert.
- Der jeweils nicht ausgewählte Ausgang behält seinen bisherigen Wert.

Eine Aktualisierung eines Adapter-Ausgangs erfolgt nur bei einer tatsächlichen Wertänderung. Das Ereignis `CNF` wird ebenfalls nur dann erzeugt und bestätigt die Übernahme des Index `K`. Dadurch werden unnötige Ereignisse und Datenübertragungen vermieden.

## Technische Besonderheiten

- Vollständig adapterbasierte Schnittstelle ohne klassische Daten-Ein-/Ausgänge.
- Verwendung unidirektionaler Adaptertypen `A2X` und `AUI`.
- Der Baustein ist als generischer Funktionsblock mit dem ClassName `GEN_A2X_AUI_DEMUX` hinterlegt.
- Die Ereignisausgabe `CNF` erfolgt nur bei tatsächlicher Änderung, nicht bei jedem Zyklus.
- Der Baustein ist dem Compiler-Paket `adapter::selection::unidirectional` zugeordnet.
- Laut Versionsinformation ist das zugehörige Adapter-Backend `GEN_A2X_AUI_DEMUX` noch zu implementieren.

## Zustandsübersicht

Eine explizite Zustandsmaschine ist in der Typdefinition nicht enthalten. Das Verhalten lässt sich konzeptionell wie folgt beschreiben:

| Zustand | Bedeutung |
|---|---|
| Warten | Es liegt keine Wertänderung vor; `OUT1` und `OUT2` behalten ihre Werte. |
| Auswahl | Der Index `K` wird ausgewertet und der zugehörige Ausgang wird mit dem Wert von `IN` aktualisiert. |
| Bestätigung | Nach der Aktualisierung wird `CNF` ausgegeben und der Baustein kehrt in den Wartezustand zurück. |

## Anwendungsszenarien

- Verteilung eines Wertes an zwei unterschiedliche Verbraucher, z. B. Steuerung und Visualisierung.
- Umschalten zwischen zwei Betriebsmodi anhand eines Auswahlindexes.
- Einsatz in IEC-61499-Systemen mit unidirektionalen Adapterverbindungen zur Reduzierung der Ereignislast.

## Vergleich mit ähnlichen Bausteinen

Gegenüber klassischen Demultiplexer-Bausteinen, die separate Daten- und Ereignisports verwenden, kapselt `A2X_AUI_DEMUX_2` die Datenschnittstellen vollständig in Adapter. Eine verwandte Variante ist `AX_AUI_DEMUX_2`, auf der dieser Baustein basiert. Der wesentliche Unterschied liegt in der Adapter-basierten Anbindung und der integrierten Änderungserkennung, durch die das Ereignis `CNF` nur bei tatsächlichen Wertänderungen ausgelöst wird.

## Fazit

`A2X_AUI_DEMUX_2` ist ein kompakter, adapter-basierter Demultiplexer für zwei unidirektionale Ausgänge. Er ermöglicht eine klare modulare Verdrahtung und vermeidet unnötige Ereignisse durch seine Änderungserkennung. Damit eignet er sich gut für selektive Wertverteilungen in IEC-61499-Anwendungen.

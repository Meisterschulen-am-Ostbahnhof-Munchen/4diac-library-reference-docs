# ASR_MERGE_3

![ASR_MERGE_3](ASR_MERGE_3.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **ASR_MERGE_3** führt 3 unidirektionale ASR-Adapter (Set/Reset-Ereignisadapter, keine Nutzdaten) auf einen gemeinsamen Ausgangsadapter zusammen. Er ist die 3-Eingangs-Variante von [ASR_MERGE_2](ASR_MERGE_2.md) und wie dieser als generischer FB implementiert (`GenericClassName: 'GEN_ASR_MERGE'`) — dieselbe C++-Basis (`CGenUnidirectMergeBase`), nur mit 3 statt 2 Sockets.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine. Die Ereignisübernahme erfolgt ausschließlich über die Adapter-Sockets.

### **Ereignis-Ausgänge**

Keine. Die Ereignisweitergabe erfolgt ausschließlich über den Adapter-Plug.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

| Rolle | Name | Typ | Beschreibung |
| ------- | ------ | ----- | -------------- |
| Socket (Eingang 1) | `IN1` | `adapter::types::unidirectional::ASR` | 1. Quelle. |
| Socket (Eingang 2) | `IN2` | `adapter::types::unidirectional::ASR` | 2. Quelle. |
| Socket (Eingang 3) | `IN3` | `adapter::types::unidirectional::ASR` | 3. Quelle. |
| Plug (Ausgang) | `OUT` | `adapter::types::unidirectional::ASR` | Zusammengeführtes Signal. |

## Funktionsweise

Trifft `SET` bzw. `RESET` an einem der 3 Sockets (`IN1`, `IN2`, `IN3`) ein, wird es als gleichartiges Ereignis unverändert an `OUT` weitergereicht. Alle Ereignistypen werden unabhängig voneinander behandelt, und alle 3 Sockets werden gleichberechtigt behandelt — welcher Socket zuerst geprüft wird, folgt der Reihenfolge `IN1` … `IN3`, was nur bei mehreren innerhalb desselben Ausführungszyklus eintreffenden Ereignissen relevant wird. Da `ASR` keinen Datenwert transportiert, gibt es nichts zu arbitrieren außer dem jeweiligen Ereignistyp selbst.

## Technische Besonderheiten

- **Generischer Typ**: Implementiert über die generische Basisklasse `CGenUnidirectMergeBase` (N Sockets, 1 Plug); die Anzahl der Eingangs-Sockets wird über den generischen Suffix (`_3`) zur Übersetzungszeit festgelegt.
- **Keine Zustände / Algorithmen**: Da `ASR` keinen Datenwert trägt, gibt es keine Änderungserkennung und kein ECC.
- **Beliebige Eingangsanzahl**: Dieselbe Implementierung deckt ASR_MERGE_2 bis ASR_MERGE_7 ab; für andere Eingangsanzahlen siehe `ASR_MERGE_2`, `ASR_MERGE_4`, `ASR_MERGE_5`, `ASR_MERGE_6`, `ASR_MERGE_7`.

## Zustandsübersicht

Der Baustein besitzt **keinen Zustandsautomaten**. Das Verhalten ist rein kombinatorisch: Jedes eingehende Ereignis an einem der 3 Sockets wird unmittelbar an `OUT` weitergereicht.

## Anwendungsszenarien

- **Zusammenführen mehrerer gleichwertiger Signalquellen** (z. B. 3 redundante oder alternative Auslösepfade) auf einen gemeinsamen ASR-Adapterausgang.
- **Vereinfachung von Netzwerken**, die sonst 3 separate Ereignisverbindungen zum selben Ziel benötigen würden.

## Vergleich mit ähnlichen Bausteinen

- **[ASR_SPLIT_2](ASR_SPLIT_2.md)**: die Umkehrrichtung für 2 Ausgänge (bei ASRT zusätzlich ASR_SPLIT_3 bis ASR_SPLIT_9 verfügbar).
- **`ASR_MERGE_2`, `ASR_MERGE_4`, `ASR_MERGE_5`, `ASR_MERGE_6`, `ASR_MERGE_7`**: dieselbe generische Implementierung mit anderer Eingangsanzahl.
- [AE_MERGE_3](AE_MERGE_3.md), [ASRT_MERGE_3](ASRT_MERGE_3.md): dieselbe Zusammenführungslogik für die jeweils anderen unidirektionalen Ereignisadapter.

## Fazit

`ASR_MERGE_3` liefert eine generisch implementierte Zusammenführung von 3 `ASR`-Ereignisquellen auf einen gemeinsamen Adapterausgang und ergänzt die `GEN_ASR_MERGE`-Familie um die 3-Eingangs-Variante.

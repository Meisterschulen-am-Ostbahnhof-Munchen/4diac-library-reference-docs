# ASRT_SPLIT_2

![ASRT_SPLIT_2](./ASRT_SPLIT_2.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **ASRT_SPLIT_2** verteilt einen eingehenden unidirektionalen ASRT-Adapter (Set/Reset/Toggle-Ereignistripel, keine Nutzdaten) auf zwei identische Ausgangsadapter. `ASRT` erweitert `ASR` um ein drittes Ereignis `TOGGLE` (siehe `AX_T_FF_SR`). Er ist als generischer FB implementiert (`GenericClassName: 'GEN_ASRT_SPLIT'`) und komplettiert die `SPLIT`-Familie neben [AE_SPLIT_2](AE_SPLIT_2.md) und [ASR_SPLIT_2](ASR_SPLIT_2.md).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine. Die Ereignisübernahme erfolgt ausschließlich über den Adapter-Socket.

### **Ereignis-Ausgänge**

Keine. Die Ereignisweitergabe erfolgt ausschließlich über die Adapter-Plugs.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

| Rolle | Name | Typ | Beschreibung |
| ------- | ------ | ----- | -------------- |
| Socket (Eingang) | `IN` | `adapter::types::unidirectional::ASRT` | Empfängt ein SET-, RESET- oder TOGGLE-Ereignis, das auf beide Ausgänge verteilt wird. |
| Plug (Ausgang 1) | `OUT1` | `adapter::types::unidirectional::ASRT` | Erster Ausgang für das duplizierte Ereignis. |
| Plug (Ausgang 2) | `OUT2` | `adapter::types::unidirectional::ASRT` | Zweiter Ausgang für das duplizierte Ereignis. |

## Funktionsweise

Sobald am Adapter-Socket `IN` ein `SET`-, `RESET`- oder `TOGGLE`-Ereignis eintrifft, wird es **sofort und parallel** als gleichartiges Ereignis an beide Ausgangs-Plugs `OUT1` und `OUT2` weitergeleitet. Der Baustein führt keinerlei Logik, Filterung oder Verzögerung durch – er fungiert als reiner Splitter auf Adapterebene, unabhängig davon, welcher der drei Ereignistypen eintrifft.

## Technische Besonderheiten

- **Generischer Typ**: Implementiert über die generische Basisklasse `CGenUnidirectSplitBase` (1 Socket, N Plugs) – dieselbe C++-Basis, die auch `GEN_AE_SPLIT` und `GEN_ASR_SPLIT` zugrunde liegt.
- **Drei unabhängige Ereigniskanäle**: `SET`, `RESET` und `TOGGLE` werden getrennt geprüft und jeweils auf beide Ausgänge verteilt.
- **Keine Zustände / Algorithmen**: Da `ASRT` keinen Datenwert trägt, gibt es keine Änderungserkennung und kein ECC.

## Zustandsübersicht

Der Baustein besitzt **keinen Zustandsautomaten**. Das Verhalten ist rein kombinatorisch: Jedes eingehende `SET`-, `RESET`- oder `TOGGLE`-Ereignis am Socket `IN` wird unmittelbar an beiden Ausgängen dupliziert.

## Anwendungsszenarien

- **Ereignisverteilung**: Ein Set/Reset/Toggle-Signal (z. B. von `AX_T_FF_SR` gesteuert) soll von zwei unabhängigen Subsystemen verarbeitet werden.
- **Parallelschaltung**: Aufteilung eines ASRT-Signals zur gleichzeitigen Ansteuerung zweier Aktoren.

## Vergleich mit ähnlichen Bausteinen

- **[ASRT_MERGE_2](ASRT_MERGE_2.md)**: die Umkehrrichtung – führt zwei eingehende Set/Reset/Toggle-Signale auf einen gemeinsamen Ausgang zusammen, statt ein Signal zu verteilen.
- **[ASR_SPLIT_2](ASR_SPLIT_2.md)**: dieselbe Verteilungslogik für den Set/Reset-Ereignisadapter `ASR` (ohne `TOGGLE`).
- **[AE_SPLIT_2](AE_SPLIT_2.md)**: dieselbe Verteilungslogik für den reinen Ereignisadapter `AE` (1 Ereignis statt SET/RESET/TOGGLE).

## Fazit

Der **ASRT_SPLIT_2** ist ein minimaler generischer Baustein zur Verteilung eines Set/Reset/Toggle-Ereignisses auf zwei ASRT-Adapterausgänge und schließt die zuletzt verbliebene Lücke in der `GEN_*_SPLIT`-Familie für alle drei unidirektionalen Ereignisadapter (`AE`, `ASR`, `ASRT`).

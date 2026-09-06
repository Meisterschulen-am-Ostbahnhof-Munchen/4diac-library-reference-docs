# ASRT_MERGE_2

![ASRT_MERGE_2](./ASRT_MERGE_2.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **ASRT_MERGE_2** führt zwei unidirektionale ASRT-Adapter (Set/Reset/Toggle-Ereignistripel, keine Nutzdaten) auf einen gemeinsamen Ausgangsadapter zusammen. `ASRT` erweitert `ASR` um ein drittes Ereignis `TOGGLE` (siehe `AX_T_FF_SR`). Der Baustein ist die Umkehrung von [ASRT_SPLIT_2](ASRT_SPLIT_2.md) und als generischer FB implementiert (`GenericClassName: 'GEN_ASRT_MERGE'`).

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
| Socket (Eingang 1) | `IN1` | `adapter::types::unidirectional::ASRT` | Erste SET/RESET/TOGGLE-Quelle. |
| Socket (Eingang 2) | `IN2` | `adapter::types::unidirectional::ASRT` | Zweite SET/RESET/TOGGLE-Quelle. |
| Plug (Ausgang) | `OUT` | `adapter::types::unidirectional::ASRT` | Zusammengeführtes SET/RESET/TOGGLE-Signal. |

## Funktionsweise

Trifft ein `SET`-, `RESET`- oder `TOGGLE`-Ereignis an `IN1` **oder** `IN2` ein, wird es als gleichartiges Ereignis an `OUT` weitergereicht. Alle drei Ereignistypen werden unabhängig voneinander behandelt – welches Ereignis an welchem Socket eintrifft, hat keinen Einfluss auf die anderen beiden Ereignistypen oder den jeweils anderen Socket. Da `ASRT` keinen Datenwert transportiert, gibt es nichts zu arbitrieren außer dem jeweiligen Ereignistyp selbst.

## Technische Besonderheiten

- **Generischer Typ**: Implementiert über die generische Basisklasse `CGenUnidirectMergeBase` (N Sockets, 1 Plug) – dieselbe C++-Basis, die auch `GEN_AE_MERGE` und `GEN_ASR_MERGE` zugrunde liegt.
- **Drei unabhängige Ereigniskanäle**: `SET`, `RESET` und `TOGGLE` werden getrennt geprüft und weitergeleitet.
- **Keine Zustände / Algorithmen**: Da `ASRT` keinen Datenwert trägt, gibt es keine Änderungserkennung und kein ECC.

## Zustandsübersicht

Der Baustein besitzt **keinen Zustandsautomaten**. Das Verhalten ist rein kombinatorisch: Jedes eingehende `SET`-, `RESET`- oder `TOGGLE`-Ereignis an `IN1` oder `IN2` wird unmittelbar als gleichartiges Ereignis an `OUT` weitergereicht.

## Anwendungsszenarien

- **Zusammenführen redundanter Set/Reset/Toggle-Quellen**: z. B. zwei Bedienstellen, die dasselbe `AX_T_FF_SR`-gesteuerte Element über einen gemeinsamen ASRT-Adapter ansteuern sollen.
- **Vereinfachung von Netzwerken**, die sonst drei separate Ereignisverbindungspaare (SET, RESET, TOGGLE) je Quelle zum selben Ziel benötigen würden.

## Vergleich mit ähnlichen Bausteinen

- **[ASRT_SPLIT_2](ASRT_SPLIT_2.md)**: die Umkehrrichtung – verteilt ein eingehendes Set/Reset/Toggle-Signal auf zwei Ausgänge, statt zwei Eingänge zusammenzuführen.
- **[ASR_MERGE_2](ASR_MERGE_2.md)**: dieselbe Zusammenführungslogik für den Set/Reset-Ereignisadapter `ASR` (ohne `TOGGLE`).
- **[AE_MERGE_2](AE_MERGE_2.md)**: dieselbe Zusammenführungslogik für den reinen Ereignisadapter `AE` (1 Ereignis statt SET/RESET/TOGGLE).

## Fazit

Der **ASRT_MERGE_2** ist ein minimaler generischer Baustein zur Zusammenführung zweier Set/Reset/Toggle-Ereignisquellen auf einen gemeinsamen ASRT-Adapterausgang und komplettiert die `GEN_*_MERGE`-Familie für alle drei unidirektionalen Ereignisadapter (`AE`, `ASR`, `ASRT`).

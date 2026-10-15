# SwitchPic_5_2

![SwitchPic_5_2_network](./SwitchPic_5_2_network.svg)

* * * * * * * * * *

## Einleitung

`SwitchPic_5_2` schaltet ein VT-Bild zwischen 5 Zuständen (Schieber-/Ventilanimation) anhand eines `iSTATE`-Werts um — sowohl auf einem normalen VT-Objekt (`Picture`) als auch gleichzeitig auf einem AUX-Objekt (dieselbe ID `Picture`).

Allgemeines Muster siehe [SwitchPic(Col)-Bausteine (gemeinsames Muster)](./SwitchPic-Bausteine.md).

![SwitchPic_5_2](SwitchPic_5_2.svg)

## Initialisierung und Status (INIT/INITO, QI/QO)

- **Ereignis-Eingang** `INIT` (`EInit`) und **Ereignis-Ausgang** `INITO` (`EInit`)
- **Daten-Eingang** `QI` (`BOOL`, Vorgabe `TRUE`, Quality Input) wird an `Q_NumericValue.QI` und `Q_NumericValueAux.QI` weitergegeben
- **Daten-Ausgang** `QO` (`BOOL`, Quality Output): UND (`AND_QO`, Typ `iec61131::booleanOperators::AND_BOOL_2`) der `QO`-Ausgänge von `Q_NumericValue` und `Q_NumericValueAux`
- **Initialisierungskette**: `INIT` → `F_MOVE.REQ`, `F_MOVE.CNF` → `Q_NumericValue.INIT`, `Q_NumericValue.INITO` → `Q_NumericValueAux.INIT`, `Q_NumericValueAux.INITO` → `INITO`
- `E_MERGE_QO` (Typ `iec61499::events::E_MERGE_3`) führt `Q_NumericValueAux.INITO`, `Q_NumericValue.CNF` und `Q_NumericValueAux.CNF` zusammen und löst `AND_QO.REQ` aus

## Zusammenfassung

5-Zustände-Gegenstück zu [`SwitchPic_2_2`](./SwitchPic_2_2.md).

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

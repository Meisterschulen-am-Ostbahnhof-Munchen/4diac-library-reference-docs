# GreenRedBackground4_AX

![GreenRedBackground4_AX_network](./GreenRedBackground4_AX_network.svg)

* * * * * * * * * *

## Einleitung

`GreenRedBackground4_AX` schaltet die VT-Hintergrundfarbe von 4 Objekten anhand eines booleschen Selector-Signals: `TRUE` → **Grün**, `FALSE` → **Rot**. Das Selector-Signal kommt über einen `AX`-Adapter-Socket (`DI1`). Die Objekt-ID wird über die Eingänge `u16ObjId, u16ObjIdA, u16ObjIdB` übergeben.

| Position | Objekt-ID-Quelle | Baustein                                            |
| -------- | ---------------- | --------------------------------------------------- |
| 1        | `u16ObjId`       | `Q_BackgroundColour` (normales Objekt)              |
| 2        | `u16ObjIdA`      | `Q_BackgroundColour` (normales Objekt)              |
| 3        | `u16ObjIdA`      | `Q_BackgroundColourAux` (Auxiliary-Function-Objekt) |
| 4        | `u16ObjIdB`      | `Q_BackgroundColour` (normales Objekt)              |

Allgemeines Muster (Selector → `AX_SEL`/`F_SEL` → `Q_BackgroundColour`) siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

![GreenRedBackground4_AX](GreenRedBackground4_AX.svg)

## Initialisierung und Status (INIT/INITO, QI/QO)

- **Ereignis-Eingang** `INIT` (`EInit`, Initialization Request) und **Ereignis-Ausgang** `INITO` (`EInit`, Initialization Confirm)
- **Daten-Eingang** `QI` (`BOOL`, Vorgabe `TRUE`, Quality Input) wird an `Q_BackgroundColour_1.QI`, `Q_BackgroundColour_2.QI`, `Q_BackgroundColour_3.QI` und `Q_BackgroundColour_4.QI` weitergegeben
- **Daten-Ausgang** `QO` (`BOOL`, Quality Output): UND (`AND_QO`, Typ `iec61131::booleanOperators::AND_BOOL_4`) der `QO`-Ausgänge von `Q_BackgroundColour_1.QO`, `Q_BackgroundColour_2.QO`, `Q_BackgroundColour_3.QO` und `Q_BackgroundColour_4.QO`
- **Initialisierungskette**: `INIT` → `Q_BackgroundColour_1.INIT`, `Q_BackgroundColour_1.INITO` → `Q_BackgroundColour_2.INIT`, `Q_BackgroundColour_2.INITO` → `Q_BackgroundColour_3.INIT`, `Q_BackgroundColour_3.INITO` → `Q_BackgroundColour_4.INIT`, `Q_BackgroundColour_4.INITO` → `INITO`
- `E_MERGE_QO_A` (Typ `iec61499::events::E_MERGE_4`) führt `Q_BackgroundColour_4.INITO`, `Q_BackgroundColour_1.CNF`, `Q_BackgroundColour_2.CNF` und `Q_BackgroundColour_3.CNF` zusammen und gibt das Ergebnis an `E_MERGE_QO.EI1`
- `E_MERGE_QO` (Typ `iec61499::events::E_MERGE_2`) führt `E_MERGE_QO_A.EO` und `Q_BackgroundColour_4.CNF` zusammen und löst `AND_QO.REQ` aus

## Zusammenfassung

Eine von vielen Varianten der Background-Farbbausteine-Familie: Farbpaar Grün/Rot, 4 Objekte, Adapter-Selector.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

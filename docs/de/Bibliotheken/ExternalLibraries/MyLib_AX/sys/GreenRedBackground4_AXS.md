# GreenRedBackground4_AXS

![GreenRedBackground4_AXS_network](./GreenRedBackground4_AXS_network.svg)

* * * * * * * * * *

## Einleitung

`GreenRedBackground4_AXS` schaltet die VT-Hintergrundfarbe von 4 Objekten anhand eines booleschen Selector-Signals: `TRUE` → **Grün**, `FALSE` → **Rot**. Das Selector-Signal kommt über einen `AX`-Adapter-Socket (`DI1`). Die Objekt-ID wird über den strukturierten Typ `s3ObjectIDs` (`u16ObjIds`) übergeben.

| Position | Objekt-ID-Quelle       | Baustein                                            |
| -------- | ---------------------- | --------------------------------------------------- |
| 1        | `F_MOVE.OUT.u16ObjId`  | `Q_BackgroundColour` (normales Objekt)              |
| 2        | `F_MOVE.OUT.u16ObjIdA` | `Q_BackgroundColour` (normales Objekt)              |
| 3        | `F_MOVE.OUT.u16ObjIdA` | `Q_BackgroundColourAux` (Auxiliary-Function-Objekt) |
| 4        | `F_MOVE.OUT.u16ObjIdB` | `Q_BackgroundColour` (normales Objekt)              |

Allgemeines Muster (Selector → `AX_SEL`/`F_SEL` → `Q_BackgroundColour`) siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

**Initialisierung und Qualität:** Die SubApp besitzt `INIT`/`INITO` sowie `QI`/`QO`. `INIT` → `Q_BackgroundColour_1.INIT` → `Q_BackgroundColour_2.INIT` → `Q_BackgroundColour_3.INIT` → `Q_BackgroundColour_4.INIT` (jeweils über `INITO`), `Q_BackgroundColour_4.INITO` → `INITO`. `QI` geht an alle vier Instanzen. `QO` ist das `AND` (`AND_QO`, `AND_BOOL_4`) der vier `QO`-Ausgänge; `AND_QO.REQ` wird über die Kaskade `E_MERGE_QO_A` (`E_MERGE_4`: `Q_BackgroundColour_4.INITO`, `Q_BackgroundColour_1.CNF`, `_2.CNF`, `_3.CNF`) und `E_MERGE_QO` (`E_MERGE_2`: Ausgang von `E_MERGE_QO_A` und `Q_BackgroundColour_4.CNF`) ausgelöst.

![GreenRedBackground4_AXS](GreenRedBackground4_AXS.svg)

## Zusammenfassung

Eine von vielen Varianten der Background-Farbbausteine-Familie: Farbpaar Grün/Rot, 4 Objekte, Adapter-Selector, struct. Objekt-ID.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

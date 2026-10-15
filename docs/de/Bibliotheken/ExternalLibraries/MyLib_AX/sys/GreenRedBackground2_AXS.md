# GreenRedBackground2_AXS

![GreenRedBackground2_AXS_network](./GreenRedBackground2_AXS_network.svg)

* * * * * * * * * *

## Einleitung

`GreenRedBackground2_AXS` schaltet die VT-Hintergrundfarbe von 2 Objekten anhand eines booleschen Selector-Signals: `TRUE` → **Grün**, `FALSE` → **Rot**. Das Selector-Signal kommt über einen `AX`-Adapter-Socket (`DI1`). Die Objekt-ID wird über den strukturierten Typ `s2ObjectIDs` (`u16ObjIds`) übergeben.

| Position | Objekt-ID-Quelle       | Baustein                                            |
| -------- | ---------------------- | --------------------------------------------------- |
| 1        | `F_MOVE.OUT.u16ObjIdA` | `Q_BackgroundColour` (normales Objekt)              |
| 2        | `F_MOVE.OUT.u16ObjIdA` | `Q_BackgroundColourAux` (Auxiliary-Function-Objekt) |

Allgemeines Muster (Selector → `AX_SEL`/`F_SEL` → `Q_BackgroundColour`) siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

**Initialisierung und Qualität:** Die SubApp besitzt `INIT`/`INITO` sowie `QI`/`QO`. `INIT` → `Q_BackgroundColour_1.INIT`, `Q_BackgroundColour_1.INITO` → `Q_BackgroundColour_2.INIT`, `Q_BackgroundColour_2.INITO` → `INITO`. `QI` geht an beide `Q_BackgroundColour`-Instanzen. `QO` ist das `AND` (`AND_QO`, `AND_BOOL_2`) der beiden `QO`-Ausgänge; `AND_QO.REQ` wird über `E_MERGE_QO` (`E_MERGE_3`: `Q_BackgroundColour_2.INITO`, `Q_BackgroundColour_1.CNF`, `Q_BackgroundColour_2.CNF`) ausgelöst.

![GreenRedBackground2_AXS](GreenRedBackground2_AXS.svg)

## Zusammenfassung

Eine von vielen Varianten der Background-Farbbausteine-Familie: Farbpaar Grün/Rot, 2 Objekte, Adapter-Selector, struct. Objekt-ID.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

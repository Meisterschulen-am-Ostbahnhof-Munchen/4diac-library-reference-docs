# RedWhiteBackground3_AX

![RedWhiteBackground3_AX_network](./RedWhiteBackground3_AX_network.svg)

* * * * * * * * * *

## Einleitung

`RedWhiteBackground3_AX` schaltet die VT-Hintergrundfarbe von 3 Objekten anhand eines booleschen Selector-Signals: `TRUE` → **Rot**, `FALSE` → **Weiß**. Das Selector-Signal kommt über einen `AX`-Adapter-Socket (`DI1`). Die Objekt-ID wird über die Eingänge `u16ObjId, u16ObjIdA` übergeben.

| Position | Objekt-ID-Quelle | Baustein                                            |
| -------- | ---------------- | --------------------------------------------------- |
| 1        | `u16ObjId`       | `Q_BackgroundColour` (normales Objekt)              |
| 2        | `u16ObjIdA`      | `Q_BackgroundColour` (normales Objekt)              |
| 3        | `u16ObjIdA`      | `Q_BackgroundColourAux` (Auxiliary-Function-Objekt) |

**Initialisierung und Qualität:** Zusätzlich zu den bisherigen Ereignissen besitzt der Baustein den Ereigniseingang `INIT`, den Ereignisausgang `INITO` sowie `QI` (BOOL, Eingang, Initialwert `TRUE`) und `QO` (BOOL, Ausgang, UND aller Instanzen). Init-Kette: `INIT` → `Q_BackgroundColour_1` → `Q_BackgroundColour_2` → `Q_BackgroundColour_3` → `INITO` (jeweils `INITO` → nächstes `INIT`). `QI` wird an alle `Q_BackgroundColour_n.QI` weitergereicht. `QO` bildet `AND_QO` (`iec61131::booleanOperators::AND_BOOL_3`): `Q_BackgroundColour_1.QO` → `AND_QO.IN1`, `Q_BackgroundColour_2.QO` → `AND_QO.IN2`, `Q_BackgroundColour_3.QO` → `AND_QO.IN3`, `AND_QO.OUT` → `QO`. `E_MERGE_QO` (`E_MERGE_4`) führt `Q_BackgroundColour_3.INITO` und die `CNF`-Ausgänge `Q_BackgroundColour_1.CNF`, `Q_BackgroundColour_2.CNF`, `Q_BackgroundColour_3.CNF` zusammen; `E_MERGE_QO.EO` triggert `AND_QO.REQ`.

Allgemeines Muster (Selector → `AX_SEL`/`F_SEL` → `Q_BackgroundColour`) siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

![RedWhiteBackground3_AX](RedWhiteBackground3_AX.svg)

## Zusammenfassung

Eine von vielen Varianten der Background-Farbbausteine-Familie: Farbpaar Rot/Weiß, 3 Objekte, Adapter-Selector.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

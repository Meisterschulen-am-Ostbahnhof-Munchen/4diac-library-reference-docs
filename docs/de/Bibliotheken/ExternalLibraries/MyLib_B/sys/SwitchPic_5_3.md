# SwitchPic_5_3

![SwitchPic_5_3_network](./SwitchPic_5_3_network.svg)

* * * * * * * * * *

## Einleitung

`SwitchPic_5_3` schaltet ein VT-Bild zwischen 5 Zuständen (Schieber-/Ventilanimation) anhand eines `iSTATE`-Werts um — auf drei unabhängigen Zielen gleichzeitig: normales Softkey-Objekt (`Picture`), AUX-Objekt (`PictureA`) und zweites normales Button-Objekt (`PictureB`), jedes mit eigener Object-ID.

Allgemeines Muster siehe [SwitchPic(Col)-Bausteine (gemeinsames Muster)](./SwitchPic-Bausteine.md).

**Initialisierung und Qualität:** `INIT` → `F_MOVE.REQ`; `F_MOVE.CNF` → `Q_NumericValue.INIT` → `Q_NumericValueAux.INIT` → `Q_NumericValue_B.INIT` (jeweils über `INITO`); `Q_NumericValue_B.INITO` → `INITO`. Neu ist `QI` (`BOOL`, Vorgabe `TRUE`), das an alle drei `Q_NumericValue`-Instanzen geht, sowie `QO` (`BOOL`): `AND` (`AND_QO`, `AND_BOOL_3`) der drei `QO`-Ausgänge. `AND_QO.REQ` wird über `E_MERGE_QO` (`E_MERGE_4`: `Q_NumericValue_B.INITO`, `Q_NumericValue.CNF`, `Q_NumericValueAux.CNF`, `Q_NumericValue_B.CNF`) ausgelöst.

![SwitchPic_5_3](SwitchPic_5_3.svg)

## Zusammenfassung

5-Zustände-Gegenstück zu [`SwitchPic_2_3`](./SwitchPic_2_3.md) — die umfangreichste Variante der `SwitchPic_5_*`-Reihe.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

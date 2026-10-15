# SwitchPicCol_5_1_aux

![SwitchPicCol_5_1_aux_network](./SwitchPicCol_5_1_aux_network.svg)

* * * * * * * * * *

## Einleitung

`SwitchPicCol_5_1_aux` schaltet zwischen 5 Zuständen sowohl ein Bild als auch eine Hintergrundfarbe um — wie [`SwitchPicCol_5_1`](./SwitchPicCol_5_1.md), aber ausschließlich für **Auxiliary-Function**-Objekte (`Q_NumericValueAux`/`Q_BackgroundColourAux`) statt normale VT-Objekte.

**Initialisierung und Qualität:** Die SubApp besitzt die Ereigniseingänge `INIT` und `EI`, den Ereignisausgang `INITO` sowie `QI` (BOOL, Initialwert `TRUE`) und `QO` (BOOL). Init-Kette: `INIT` → `F_MOVE.REQ`, `F_MOVE.CNF` → `PictureAux.INIT`, `PictureAux.INITO` → `ColorAux.INIT`, `ColorAux.INITO` → `INITO`. `QI` wird an `PictureAux.QI` und `ColorAux.QI` weitergereicht; deren `QO`-Ausgänge gehen auf `AND_QO.IN1` bzw. `AND_QO.IN2` (`iec61131::booleanOperators::AND_BOOL_2`), `AND_QO.OUT` → `QO`. `AND_QO.REQ` wird von `E_MERGE_QO` (`E_MERGE_3`) getriggert, das `ColorAux.INITO`, `PictureAux.CNF` und `ColorAux.CNF` zusammenführt.

Allgemeines Muster siehe [SwitchPic(Col)-Bausteine (gemeinsames Muster)](./SwitchPic-Bausteine.md).

![SwitchPicCol_5_1_aux](SwitchPicCol_5_1_aux.svg)

## Zusammenfassung

AUX-Gegenstück zu [`SwitchPicCol_5_1`](./SwitchPicCol_5_1.md).

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

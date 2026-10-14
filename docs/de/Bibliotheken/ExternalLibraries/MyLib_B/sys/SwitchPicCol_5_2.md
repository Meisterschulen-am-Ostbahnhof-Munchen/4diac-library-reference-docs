# SwitchPicCol_5_2

![SwitchPicCol_5_2_network](./SwitchPicCol_5_2_network.svg)

* * * * * * * * * *

## Einleitung

`SwitchPicCol_5_2` schaltet zwischen 5 Zuständen sowohl Bild als auch Hintergrundfarbe um — auf **vier** Zielen gleichzeitig: normales Bild-Objekt (`PictureVT`), AUX-Bild-Objekt (`PictureAux`), normales Farb-Objekt (`ColorVT`) und AUX-Farb-Objekt (`ColorAux`), alle über dieselben zwei `F_MUX_5`-Multiplexer (Bild/Farbe) gesteuert.

Allgemeines Muster siehe [SwitchPic(Col)-Bausteine (gemeinsames Muster)](./SwitchPic-Bausteine.md).

![SwitchPicCol_5_2](SwitchPicCol_5_2.svg)

## Initialisierung und Qualität

Der Baustein besitzt den Ereigniseingang `INIT`, den Ereignisausgang `INITO` (je `EInit`), den Dateneingang `QI` (`BOOL`, Default `TRUE`) und den Datenausgang `QO` (`BOOL`). Die Initialisierung läuft als Kette: `INIT` → `F_MOVE.REQ`, `F_MOVE.CNF` → `PictureAux.INIT`, `PictureAux.INITO` → `PictureVT.INIT`, `PictureVT.INITO` → `ColorAux.INIT`, `ColorAux.INITO` → `ColorVT.INIT`, `ColorVT.INITO` → `INITO`. `QI` geht an `PictureAux`, `PictureVT`, `ColorAux` und `ColorVT`. `QO` ist der Ausgang der Instanz `AND_QO` (`iec61131::booleanOperators::AND_BOOL_4`), die deren vier `QO` verknüpft. Ihr `REQ` wird von `E_MERGE_QO` (`E_MERGE_2`) ausgelöst, das den Ausgang von `E_MERGE_QO_A` (`E_MERGE_4`, führt die `CNF` von `PictureAux`, `PictureVT`, `ColorAux` und `ColorVT` zusammen) und `ColorVT.INITO` zusammenführt.

## Zusammenfassung

Erweiterung von [`SwitchPicCol_5_1`](./SwitchPicCol_5_1.md) um die AUX-Objekte für Bild und Farbe gleichzeitig — die umfangreichste Variante der `SwitchPicCol`-Reihe.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

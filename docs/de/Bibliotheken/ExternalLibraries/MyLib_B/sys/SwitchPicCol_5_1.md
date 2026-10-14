# SwitchPicCol_5_1

![SwitchPicCol_5_1_network](./SwitchPicCol_5_1_network.svg)

* * * * * * * * * *

## Einleitung

`SwitchPicCol_5_1` schaltet zwischen 5 Zuständen (Schieber-/Ventilanimation) sowohl ein VT-Bild (`Picture`, via `Q_NumericValue`) **als auch** eine VT-Hintergrundfarbe (`Color`, via `Q_BackgroundColour`) um — zwei parallele `F_MUX_5`-Multiplexer (einer für Bild-IDs, einer für Farbwerte), beide vom selben `iSTATE`-Wert gesteuert, auf normale (nicht-AUX) Objekte.

Allgemeines Muster siehe [SwitchPic(Col)-Bausteine (gemeinsames Muster)](./SwitchPic-Bausteine.md).

**Initialisierung und Qualität:** `INIT` → `F_MOVE.REQ`; `F_MOVE.CNF` → `PictureVT.INIT`; `PictureVT.INITO` → `ColorVT.INIT`; `ColorVT.INITO` → `INITO`. Neu ist `QI` (`BOOL`, Vorgabe `TRUE`), das an `PictureVT.QI` und `ColorVT.QI` geht, sowie `QO` (`BOOL`): `AND` (`AND_QO`, `AND_BOOL_2`) aus `PictureVT.QO` und `ColorVT.QO`. `AND_QO.REQ` wird über `E_MERGE_QO` (`E_MERGE_3`: `ColorVT.INITO`, `PictureVT.CNF`, `ColorVT.CNF`) ausgelöst.

![SwitchPicCol_5_1](SwitchPicCol_5_1.svg)

## Zusammenfassung

Kombiniert Bild- und Farbumschaltung (`Col`) für 5 Zustände auf normalen VT-Objekten.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

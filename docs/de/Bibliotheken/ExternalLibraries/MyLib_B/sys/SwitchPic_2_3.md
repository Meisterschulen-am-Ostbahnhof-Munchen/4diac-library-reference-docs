# SwitchPic_2_3

![SwitchPic_2_3_network](./SwitchPic_2_3_network.svg)

* * * * * * * * * *

## Einleitung

`SwitchPic_2_3` schaltet ein VT-Bild zwischen 2 Zuständen (`up`/`down`) anhand eines booleschen Selectors (`DI1`) um — auf drei Zielen gleichzeitig: einem normalen Softkey-Objekt (`Picture`), einem AUX-Objekt (dieselbe ID `Picture`) und einem zweiten, unabhängigen normalen Objekt ("Button", `PictureB`, eigene Object-ID-Untermenge `u16ObjIdA` in der Struktur).

Schnittstelle (Quality-Muster): Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`) wird an `Q_NumericValue`, `Q_NumericValueAux` und `Q_NumericValue_B` verteilt; der Datenausgang `QO` (`BOOL`) ist die UND-Verknüpfung (`AND_QO`: `iec61131::booleanOperators::AND_BOOL_3`) der `QO`-Ausgänge dieser drei Instanzen (`IN1` bis `IN3` in dieser Reihenfolge). Den Trigger `AND_QO.REQ` liefert `E_MERGE_QO` (`E_MERGE_4`), der `Q_NumericValue_B.INITO` sowie die `CNF` von `Q_NumericValue`, `Q_NumericValueAux` und `Q_NumericValue_B` zusammenführt.

Allgemeines Muster siehe [SwitchPic(Col)-Bausteine (gemeinsames Muster)](./SwitchPic-Bausteine.md).

![SwitchPic_2_3](SwitchPic_2_3.svg)

## Zusammenfassung

Variante "3" (normales Softkey-Objekt + AUX-Objekt + zweites normales Button-Objekt) der 2-Zustände-Bildumschaltung — die umfangreichste Variante der `SwitchPic_2_*`-Reihe.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

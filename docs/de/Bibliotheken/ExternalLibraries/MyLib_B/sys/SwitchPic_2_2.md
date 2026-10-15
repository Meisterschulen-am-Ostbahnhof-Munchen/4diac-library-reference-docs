# SwitchPic_2_2

![SwitchPic_2_2_network](./SwitchPic_2_2_network.svg)

* * * * * * * * * *

## Einleitung

`SwitchPic_2_2` schaltet ein VT-Bild zwischen 2 Zuständen (`up`/`down`) anhand eines booleschen Selectors (`DI1`) um — sowohl auf einem normalen VT-Objekt (`Picture`, `Q_NumericValue`) als auch gleichzeitig auf einem **Auxiliary-Function**-Objekt (`Q_NumericValueAux`), beide mit derselben Object-ID (`Picture`).

Allgemeines Muster siehe [SwitchPic(Col)-Bausteine (gemeinsames Muster)](./SwitchPic-Bausteine.md).

![SwitchPic_2_2](SwitchPic_2_2.svg)

## Initialisierung und Qualität

- **Ereignis-Eingänge** `INIT` (`EInit`) und `EI` (Umschaltanforderung); **Ereignis-Ausgang** `INITO` (`EInit`).
- **Dateneingang `QI`** (`BOOL`, Vorgabe `TRUE`) → `Q_NumericValue.QI` und `Q_NumericValueAux.QI`; **Datenausgang `QO`** (`BOOL`): UND aus `Q_NumericValue.QO` und `Q_NumericValueAux.QO` (`AND_QO`, `iec61131::booleanOperators::AND_BOOL_2`).
- Initialisierung: `INIT` → `F_MOVE.REQ`; `F_MOVE.CNF` → `Q_NumericValue.INIT`; `Q_NumericValue.INITO` → `Q_NumericValueAux.INIT`; `Q_NumericValueAux.INITO` → `INITO`.
- Der `AND_QO.REQ`-Trigger kommt von `E_MERGE_QO` (`iec61499::events::E_MERGE_3`), das `Q_NumericValueAux.INITO`, `Q_NumericValue.CNF` und `Q_NumericValueAux.CNF` vereint.

## Zusammenfassung

Variante "2" (normales Objekt + AUX-Objekt) der 2-Zustände-Bildumschaltung.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

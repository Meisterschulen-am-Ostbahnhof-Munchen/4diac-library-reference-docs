# RedWhiteBackground2_AXC

![RedWhiteBackground2_AXC_network](./RedWhiteBackground2_AXC_network.svg)

![RedWhiteBackground2_AXC](./RedWhiteBackground2_AXC.svg)

* * * * * * * * * *

## Einleitung

`RedWhiteBackground2_AXC` ist ein **Kompakt-Wrapper** um [`RedWhiteBackground2_AX`](RedWhiteBackground2_AX.md) — reicht `u16ObjId` und den Selector durch, ohne die `CNF`-Ereignisausgänge und Diagnosewerte (`STATUS_n`, `u8OldColour_n`, `result_n`) nach außen zu führen.

Schnittstelle (Quality-Muster): Der Wrapper besitzt zusätzlich den Ereignis-Eingang `INIT` und den Ereignis-Ausgang `INITO` (je `EInit`) sowie den Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`) und den Datenausgang `QO` (`BOOL`). Sie sind direkt an die innere Instanz `RedWhiteBackground2` durchgeschleift: `INIT` → `RedWhiteBackground2.INIT`, `RedWhiteBackground2.INITO` → `INITO`, `QI` → `RedWhiteBackground2.QI`, `RedWhiteBackground2.QO` → `QO`.

Details zum gemeinsamen Muster siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

## Zusammenfassung

Reine Weiterleitung an `RedWhiteBackground2_AX` für den Regelfall, in dem die Diagnosewerte nicht gebraucht werden.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

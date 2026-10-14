# RedGreenBackground4SC

![RedGreenBackground4SC_network](./RedGreenBackground4SC_network.svg)

![RedGreenBackground4SC](./RedGreenBackground4SC.svg)

* * * * * * * * * *

## Einleitung

`RedGreenBackground4SC` ist ein **Kompakt-Wrapper** um [`RedGreenBackground4S`](RedGreenBackground4S.md) — reicht `u16ObjId`s und den Selector durch, ohne die `CNF`-Ereignisausgänge und Diagnosewerte (`STATUS_n`, `u8OldColour_n`, `result_n`) nach außen zu führen.

Schnittstelle (Quality-Muster): Der Wrapper besitzt zusätzlich den Ereignis-Eingang `INIT` und den Ereignis-Ausgang `INITO` (je `EInit`) sowie den Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`) und den Datenausgang `QO` (`BOOL`). Sie sind direkt an die innere Instanz `RedGreenBackground` durchgeschleift: `INIT` → `RedGreenBackground.INIT`, `RedGreenBackground.INITO` → `INITO`, `QI` → `RedGreenBackground.QI`, `RedGreenBackground.QO` → `QO`.

Details zum gemeinsamen Muster siehe [Background-Farbbausteine (gemeinsames Muster)](../../MyLib_AX/sys/Background-Farbbausteine.md).

## Zusammenfassung

Reine Weiterleitung an `RedGreenBackground4S` für den Regelfall, in dem die Diagnosewerte nicht gebraucht werden.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

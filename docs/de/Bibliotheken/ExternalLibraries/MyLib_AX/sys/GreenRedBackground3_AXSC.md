# GreenRedBackground3_AXSC

![GreenRedBackground3_AXSC_network](./GreenRedBackground3_AXSC_network.svg)

![GreenRedBackground3_AXSC](./GreenRedBackground3_AXSC.svg)

* * * * * * * * * *

## Einleitung

`GreenRedBackground3_AXSC` ist ein **Kompakt-Wrapper** um [`GreenRedBackground3_AXS`](GreenRedBackground3_AXS.md) — reicht `u16ObjId`s und den Selector durch, ohne die `CNF`-Ereignisausgänge und Diagnosewerte (`STATUS_n`, `u8OldColour_n`, `result_n`) nach außen zu führen.

Schnittstelle (Quality-Muster): Der Wrapper besitzt zusätzlich den Ereignis-Eingang `INIT` und den Ereignis-Ausgang `INITO` (je `EInit`) sowie den Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`) und den Datenausgang `QO` (`BOOL`). Sie sind direkt an die innere Instanz `GreenRedBackground` durchgeschleift: `INIT` → `GreenRedBackground.INIT`, `GreenRedBackground.INITO` → `INITO`, `QI` → `GreenRedBackground.QI`, `GreenRedBackground.QO` → `QO`.

Details zum gemeinsamen Muster siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

## Zusammenfassung

Reine Weiterleitung an `GreenRedBackground3_AXS` für den Regelfall, in dem die Diagnosewerte nicht gebraucht werden.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

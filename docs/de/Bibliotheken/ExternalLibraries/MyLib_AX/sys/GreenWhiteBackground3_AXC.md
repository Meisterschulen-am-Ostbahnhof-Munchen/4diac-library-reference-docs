# GreenWhiteBackground3_AXC

![GreenWhiteBackground3_AXC_network](./GreenWhiteBackground3_AXC_network.svg)

![GreenWhiteBackground3_AXC](./GreenWhiteBackground3_AXC.svg)

* * * * * * * * * *

## Einleitung

`GreenWhiteBackground3_AXC` ist ein **Kompakt-Wrapper** um [`GreenWhiteBackground3_AX`](GreenWhiteBackground3_AX.md) — reicht `u16ObjId` und den Selector durch, ohne die `CNF`-Ereignisausgänge und Diagnosewerte (`STATUS_n`, `u8OldColour_n`, `result_n`) nach außen zu führen.

Schnittstelle (Quality-Muster): Der Wrapper besitzt zusätzlich den Ereignis-Eingang `INIT` und den Ereignis-Ausgang `INITO` (je `EInit`) sowie den Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`) und den Datenausgang `QO` (`BOOL`). Sie sind direkt an die innere Instanz `GreenWhiteBackground3` durchgeschleift: `INIT` → `GreenWhiteBackground3.INIT`, `GreenWhiteBackground3.INITO` → `INITO`, `QI` → `GreenWhiteBackground3.QI`, `GreenWhiteBackground3.QO` → `QO`.

Details zum gemeinsamen Muster siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

## Zusammenfassung

Reine Weiterleitung an `GreenWhiteBackground3_AX` für den Regelfall, in dem die Diagnosewerte nicht gebraucht werden.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

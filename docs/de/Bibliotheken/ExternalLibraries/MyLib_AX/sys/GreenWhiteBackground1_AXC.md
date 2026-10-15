# GreenWhiteBackground1_AXC

![GreenWhiteBackground1_AXC_network](./GreenWhiteBackground1_AXC_network.svg)

![GreenWhiteBackground1_AXC](./GreenWhiteBackground1_AXC.svg)

* * * * * * * * * *

## Einleitung

`GreenWhiteBackground1_AXC` ist ein **Kompakt-Wrapper** um [`GreenWhiteBackground1_AX`](GreenWhiteBackground1_AX.md) — reicht `u16ObjId` und den Selector durch, ohne die `CNF`-Ereignisausgänge und Diagnosewerte (`STATUS_n`, `u8OldColour_n`, `result_n`) nach außen zu führen.

Details zum gemeinsamen Muster siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

Zusätzlich führt der Wrapper die Schnittstelle für Initialisierung und Qualität nach außen: Ereigniseingang `INIT`, Ereignisausgang `INITO`, Dateneingang `QI` (`BOOL`, Initialwert `TRUE`) und Datenausgang `QO` (`BOOL`). Sie sind 1:1 an die innere Instanz `GreenWhiteBackground1` durchgeschleift: `INIT` → `GreenWhiteBackground1.INIT`, `GreenWhiteBackground1.INITO` → `INITO`, `QI` → `GreenWhiteBackground1.QI`, `GreenWhiteBackground1.QO` → `QO`.

## Zusammenfassung

Reine Weiterleitung an `GreenWhiteBackground1_AX` für den Regelfall, in dem die Diagnosewerte nicht gebraucht werden.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

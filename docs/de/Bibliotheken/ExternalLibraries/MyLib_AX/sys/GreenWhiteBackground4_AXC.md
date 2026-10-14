# GreenWhiteBackground4_AXC

![GreenWhiteBackground4_AXC_network](./GreenWhiteBackground4_AXC_network.svg)

![GreenWhiteBackground4_AXC](./GreenWhiteBackground4_AXC.svg)

* * * * * * * * * *

## Einleitung

`GreenWhiteBackground4_AXC` ist ein **Kompakt-Wrapper** um [`GreenWhiteBackground4_AX`](GreenWhiteBackground4_AX.md) — reicht `u16ObjId` und den Selector durch, ohne die `CNF`-Ereignisausgänge und Diagnosewerte (`STATUS_n`, `u8OldColour_n`, `result_n`) nach außen zu führen.

Details zum gemeinsamen Muster siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

Zusätzlich führt der Wrapper die Schnittstelle für Initialisierung und Qualität nach außen: Ereigniseingang `INIT`, Ereignisausgang `INITO`, Dateneingang `QI` (`BOOL`, Initialwert `TRUE`) und Datenausgang `QO` (`BOOL`). Sie sind 1:1 an die innere Instanz `GreenWhiteBackground4` durchgeschleift: `INIT` → `GreenWhiteBackground4.INIT`, `GreenWhiteBackground4.INITO` → `INITO`, `QI` → `GreenWhiteBackground4.QI`, `GreenWhiteBackground4.QO` → `QO`.

## Zusammenfassung

Reine Weiterleitung an `GreenWhiteBackground4_AX` für den Regelfall, in dem die Diagnosewerte nicht gebraucht werden.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

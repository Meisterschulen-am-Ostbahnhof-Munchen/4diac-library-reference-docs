# GreenWhiteBackground2_AXC

![GreenWhiteBackground2_AXC_network](./GreenWhiteBackground2_AXC_network.svg)

![GreenWhiteBackground2_AXC](./GreenWhiteBackground2_AXC.svg)

* * * * * * * * * *

## Einleitung

`GreenWhiteBackground2_AXC` ist ein **Kompakt-Wrapper** um [`GreenWhiteBackground2_AX`](GreenWhiteBackground2_AX.md) — reicht `u16ObjId` und den Selector durch, ohne die `CNF`-Ereignisausgänge und Diagnosewerte (`STATUS_n`, `u8OldColour_n`, `result_n`) nach außen zu führen. Zusätzlich werden `INIT` → `GreenWhiteBackground2.INIT`, `GreenWhiteBackground2.INITO` → `INITO`, `QI` → `GreenWhiteBackground2.QI` (Initialwert `TRUE`) und `GreenWhiteBackground2.QO` → `QO` durchgereicht.

Details zum gemeinsamen Muster siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

## Zusammenfassung

Reine Weiterleitung an `GreenWhiteBackground2_AX` für den Regelfall, in dem die Diagnosewerte nicht gebraucht werden.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

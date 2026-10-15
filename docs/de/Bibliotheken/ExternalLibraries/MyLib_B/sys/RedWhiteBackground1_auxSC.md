# RedWhiteBackground1_auxSC

![RedWhiteBackground1_auxSC_network](./RedWhiteBackground1_auxSC_network.svg)

![RedWhiteBackground1_auxSC](./RedWhiteBackground1_auxSC.svg)

* * * * * * * * * *

## Einleitung

`RedWhiteBackground1_auxSC` ist ein **Kompakt-Wrapper** um [`RedWhiteBackground1_auxS`](RedWhiteBackground1_auxS.md) — reicht `u16ObjId`s und den Selector durch, ohne die `CNF`-Ereignisausgänge und Diagnosewerte (`STATUS_n`, `u8OldColour_n`, `result_n`) nach außen zu führen. Zusätzlich werden `INIT` → `RedWhiteBackground.INIT`, `RedWhiteBackground.INITO` → `INITO`, `QI` → `RedWhiteBackground.QI` (Initialwert `TRUE`) und `RedWhiteBackground.QO` → `QO` durchgereicht.

Details zum gemeinsamen Muster siehe [Background-Farbbausteine (gemeinsames Muster)](../../MyLib_AX/sys/Background-Farbbausteine.md).

## Zusammenfassung

Reine Weiterleitung an `RedWhiteBackground1_auxS` für den Regelfall, in dem die Diagnosewerte nicht gebraucht werden.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

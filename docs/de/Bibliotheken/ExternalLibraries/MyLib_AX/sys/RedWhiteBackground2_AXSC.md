# RedWhiteBackground2_AXSC

![RedWhiteBackground2_AXSC_network](./RedWhiteBackground2_AXSC_network.svg)

![RedWhiteBackground2_AXSC](./RedWhiteBackground2_AXSC.svg)

* * * * * * * * * *

## Einleitung

`RedWhiteBackground2_AXSC` ist ein **Kompakt-Wrapper** um [`RedWhiteBackground2_AXS`](RedWhiteBackground2_AXS.md) — reicht `u16ObjId`s und den Selector durch, ohne die `CNF`-Ereignisausgänge und Diagnosewerte (`STATUS_n`, `u8OldColour_n`, `result_n`) nach außen zu führen. Zusätzlich werden `INIT` → `RedWhiteBackground.INIT`, `RedWhiteBackground.INITO` → `INITO`, `QI` → `RedWhiteBackground.QI` (Initialwert `TRUE`) und `RedWhiteBackground.QO` → `QO` durchgereicht.

Details zum gemeinsamen Muster siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

## Zusammenfassung

Reine Weiterleitung an `RedWhiteBackground2_AXS` für den Regelfall, in dem die Diagnosewerte nicht gebraucht werden.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

# RedWhiteBackground1_aux_AXSC

![RedWhiteBackground1_aux_AXSC_network](./RedWhiteBackground1_aux_AXSC_network.svg)

![RedWhiteBackground1_aux_AXSC](./RedWhiteBackground1_aux_AXSC.svg)

* * * * * * * * * *

## Einleitung

`RedWhiteBackground1_aux_AXSC` ist ein **Kompakt-Wrapper** um [`RedWhiteBackground1_aux_AXS`](RedWhiteBackground1_aux_AXS.md) — reicht `u16ObjId`s und den Selector durch, ohne die `CNF`-Ereignisausgänge und Diagnosewerte (`STATUS_n`, `u8OldColour_n`, `result_n`) nach außen zu führen.

Details zum gemeinsamen Muster siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

Zusätzlich führt der Wrapper die Schnittstelle für Initialisierung und Qualität nach außen: Ereigniseingang `INIT`, Ereignisausgang `INITO`, Dateneingang `QI` (`BOOL`, Initialwert `TRUE`) und Datenausgang `QO` (`BOOL`). Sie sind 1:1 an die innere Instanz `RedWhiteBackground` durchgeschleift: `INIT` → `RedWhiteBackground.INIT`, `RedWhiteBackground.INITO` → `INITO`, `QI` → `RedWhiteBackground.QI`, `RedWhiteBackground.QO` → `QO`.

## Zusammenfassung

Reine Weiterleitung an `RedWhiteBackground1_aux_AXS` für den Regelfall, in dem die Diagnosewerte nicht gebraucht werden.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

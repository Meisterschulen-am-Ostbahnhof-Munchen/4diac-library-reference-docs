# GreenBlueBackground1_AXSC

![GreenBlueBackground1_AXSC_network](./GreenBlueBackground1_AXSC_network.svg)

![GreenBlueBackground1_AXSC](./GreenBlueBackground1_AXSC.svg)

* * * * * * * * * *

## Einleitung

`GreenBlueBackground1_AXSC` ist ein **Kompakt-Wrapper** um [`GreenBlueBackground1_AXS`](GreenBlueBackground1_AXS.md) — reicht `u16ObjId`s und den Selector durch, ohne die `CNF`-Ereignisausgänge und Diagnosewerte (`STATUS_n`, `u8OldColour_n`, `result_n`) nach außen zu führen.

Zusätzlich werden Initialisierung und Qualität durchgereicht: `INIT` → `GreenBlueBackground.INIT`, `GreenBlueBackground.INITO` → `INITO`, `QI` (Initialwert `TRUE`) → `GreenBlueBackground.QI` und `GreenBlueBackground.QO` → `QO`.

Details zum gemeinsamen Muster siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

## Zusammenfassung

Reine Weiterleitung an `GreenBlueBackground1_AXS` für den Regelfall, in dem die Diagnosewerte nicht gebraucht werden.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

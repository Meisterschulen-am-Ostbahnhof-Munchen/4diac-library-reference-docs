# GreenRedBackground1_AXSC

![GreenRedBackground1_AXSC_network](./GreenRedBackground1_AXSC_network.svg)

![GreenRedBackground1_AXSC](./GreenRedBackground1_AXSC.svg)

* * * * * * * * * *

## Einleitung

`GreenRedBackground1_AXSC` ist ein **Kompakt-Wrapper** um [`GreenRedBackground1_AXS`](GreenRedBackground1_AXS.md) — reicht `u16ObjId`s und den Selector durch, ohne die `CNF`-Ereignisausgänge und Diagnosewerte (`STATUS_n`, `u8OldColour_n`, `result_n`) nach außen zu führen.

Details zum gemeinsamen Muster siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

## Initialisierung und Qualität

- **Ereignis-Eingang `INIT`** (`EInit`) und **Ereignis-Ausgang `INITO`** (`EInit`) werden durchgereicht: `INIT` → `GreenRedBackground.INIT`; `GreenRedBackground.INITO` → `INITO`.
- **Dateneingang `QI`** (`BOOL`, Vorgabe `TRUE`) → `GreenRedBackground.QI`; **Datenausgang `QO`** (`BOOL`) ← `GreenRedBackground.QO`.

Die innere SubApp-Instanz `GreenRedBackground` ist vom Typ `GreenRedBackground1_AXS`.

## Zusammenfassung

Reine Weiterleitung an `GreenRedBackground1_AXS` für den Regelfall, in dem die Diagnosewerte nicht gebraucht werden.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

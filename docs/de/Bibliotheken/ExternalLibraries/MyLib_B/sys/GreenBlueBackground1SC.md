# GreenBlueBackground1SC

![GreenBlueBackground1SC_network](./GreenBlueBackground1SC_network.svg)

![GreenBlueBackground1SC](./GreenBlueBackground1SC.svg)

* * * * * * * * * *

## Einleitung

`GreenBlueBackground1SC` ist ein **Kompakt-Wrapper** um [`GreenBlueBackground1S`](GreenBlueBackground1S.md) — reicht `u16ObjId`s und den Selector durch, ohne die `CNF`-Ereignisausgänge und Diagnosewerte (`STATUS_n`, `u8OldColour_n`, `result_n`) nach außen zu führen.

Details zum gemeinsamen Muster siehe [Background-Farbbausteine (gemeinsames Muster)](../../MyLib_AX/sys/Background-Farbbausteine.md).

## Initialisierung und Status (INIT/INITO, QI/QO)

- **Ereignis-Eingang** `INIT` (`EInit`, Initialization Request) und **Ereignis-Ausgang** `INITO` (`EInit`, Initialization Confirm)
- **Daten-Eingang** `QI` (`BOOL`, Vorgabe `TRUE`, Quality Input) wird an `GreenBlueBackground.QI` weitergegeben
- **Daten-Ausgang** `QO` (`BOOL`, Quality Output): durchgereicht von `GreenBlueBackground.QO`
- **Initialisierungskette**: `INIT` → `GreenBlueBackground.INIT`, `GreenBlueBackground.INITO` → `INITO`

## Zusammenfassung

Reine Weiterleitung an `GreenBlueBackground1S` für den Regelfall, in dem die Diagnosewerte nicht gebraucht werden.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

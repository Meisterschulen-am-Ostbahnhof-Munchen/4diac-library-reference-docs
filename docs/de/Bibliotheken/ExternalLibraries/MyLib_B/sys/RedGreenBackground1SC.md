# RedGreenBackground1SC

![RedGreenBackground1SC_network](./RedGreenBackground1SC_network.svg)

![RedGreenBackground1SC](./RedGreenBackground1SC.svg)

* * * * * * * * * *

## Einleitung

`RedGreenBackground1SC` ist ein **Kompakt-Wrapper** um [`RedGreenBackground1S`](RedGreenBackground1S.md) — reicht `u16ObjId`s und den Selector durch, ohne die `CNF`-Ereignisausgänge und Diagnosewerte (`STATUS_n`, `u8OldColour_n`, `result_n`) nach außen zu führen.

Details zum gemeinsamen Muster siehe [Background-Farbbausteine (gemeinsames Muster)](../../MyLib_AX/sys/Background-Farbbausteine.md).

## Initialisierung und Status (INIT/INITO, QI/QO)

- **Ereignis-Eingang** `INIT` (`EInit`, Initialization Request) und **Ereignis-Ausgang** `INITO` (`EInit`, Initialization Confirm)
- **Daten-Eingang** `QI` (`BOOL`, Vorgabe `TRUE`, Quality Input) wird an `RedGreenBackground.QI` weitergegeben
- **Daten-Ausgang** `QO` (`BOOL`, Quality Output): durchgereicht von `RedGreenBackground.QO`
- **Initialisierungskette**: `INIT` → `RedGreenBackground.INIT`, `RedGreenBackground.INITO` → `INITO`

## Zusammenfassung

Reine Weiterleitung an `RedGreenBackground1S` für den Regelfall, in dem die Diagnosewerte nicht gebraucht werden.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

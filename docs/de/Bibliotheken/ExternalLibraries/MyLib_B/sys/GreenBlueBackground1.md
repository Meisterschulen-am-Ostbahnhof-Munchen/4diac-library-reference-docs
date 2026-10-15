# GreenBlueBackground1

![GreenBlueBackground1_network](./GreenBlueBackground1_network.svg)

* * * * * * * * * *

## Einleitung

`GreenBlueBackground1` schaltet die VT-Hintergrundfarbe eines Objekts anhand eines booleschen Selector-Signals: `TRUE` → **Blau**, `FALSE` → **Weiß**. Das Selector-Signal kommt als einfacher `BOOL`-Dateneingang (`DI1`). Die Objekt-ID wird über den Eingang `u16ObjId` übergeben.

Allgemeines Muster (Selector → `AX_SEL`/`F_SEL` → `Q_BackgroundColour`) siehe [Background-Farbbausteine (gemeinsames Muster)](../../MyLib_AX/sys/Background-Farbbausteine.md).

![GreenBlueBackground1](GreenBlueBackground1.svg)

## Schnittstelle: Initialisierung und Qualität

- **INIT** (Ereignis-Eingang, `EInit`) → `Q_BackgroundColour.INIT`; `Q_BackgroundColour.INITO` → **INITO** (Ereignis-Ausgang, `EInit`).
- **QI** (Daten-Eingang, `BOOL`, Vorgabe `TRUE`): Quality Input – `TRUE` aktiviert den Dienst; durchgereicht an `Q_BackgroundColour.QI`.
- **QO** (Daten-Ausgang, `BOOL`): Quality Output – `TRUE`, wenn der Dienst erfolgreich ausgeführt wurde; kommt von `Q_BackgroundColour.QO`.

## Zusammenfassung

Eine von vielen Varianten der Background-Farbbausteine-Familie: Farbpaar Blau/Weiß, 1 Objekt, BOOL-Selector.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

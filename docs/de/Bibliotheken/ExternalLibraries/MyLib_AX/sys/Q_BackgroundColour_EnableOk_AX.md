# Q_BackgroundColour_EnableOk_AX

![Q_BackgroundColour_EnableOk_AX_network](./Q_BackgroundColour_EnableOk_AX_network.svg)

* * * * * * * * * *

## Einleitung

`Q_BackgroundColour_EnableOk_AX` entspricht `Q_BackgroundColour_EnableOk`, aber `bEnable`/`bOk` kommen als AX-Adapter (statt Klartext-BOOL) - Stil/Schnittstelle wie die bestehende GreenBlueBackground1_AX/GreenRedBackground1_AX-Familie: nur `CNF` (kein `REQ`), zusaetzlich `INIT`/`INITO` sowie `QI`/`QO`, ausserdem `STATUS_1`/`u8OldColour_1`/`result_1` als benannte Ausgaenge.

![Q_BackgroundColour_EnableOk_AX](Q_BackgroundColour_EnableOk_AX.svg)

## Verwendete Funktionsbausteine (FBs)

- **Select_EnableOk_Colour** (SubApp, Typ `MyLib::sys::Select_EnableOk_AX`): `valDisabled=COLOR_WHITE`, `valOk=COLOR_GREEN`, `valFault=COLOR_RED`.
- **Q_BackgroundColour** (`isobus::UT::Q::Q_BackgroundColour_AUS`): AUS-Adapter-Variante, schreibt die Farbe auf `u16ObjId` und liefert `u8OldColour_1` (alte Farbe) als AUS-Adapter zurueck.

## Qualitätsanschlüsse (QI/QO)

Der Baustein besitzt den Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`, Quality Input - TRUE aktiviert den Dienst) und den Datenausgang `QO` (`BOOL`, Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde). `QI` ist mit `Q_BackgroundColour.QI` verbunden, `Q_BackgroundColour.QO` mit `QO`.

Zusätzlich besitzt der Baustein die Ereignisse `INIT` (Eingang, `EInit`, Initialisierungsanforderung) und `INITO` (Ausgang, `EInit`, Initialisierungsbestätigung): `INIT` → `Q_BackgroundColour.INIT`, `Q_BackgroundColour.INITO` → `INITO`.

## Zusammenfassung

AX-Adapter-Variante von [`Q_BackgroundColour_EnableOk`](../../MyLib_B/sys/Q_BackgroundColour_EnableOk.md), passend zur bestehenden GreenBlueBackground1_AX/GreenRedBackground1_AX-Familie.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

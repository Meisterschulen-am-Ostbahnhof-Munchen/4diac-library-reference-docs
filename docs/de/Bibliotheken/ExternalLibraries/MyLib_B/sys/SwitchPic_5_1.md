# SwitchPic_5_1

![SwitchPic_5_1_network](./SwitchPic_5_1_network.svg)

* * * * * * * * * *

## Einleitung

`SwitchPic_5_1` schaltet ein VT-Bild zwischen 5 Zuständen (Schieber-/Ventilanimation: `Unknown`, `Closed`, `Opening`, `Opened`, `Closing`) anhand eines `iSTATE`-Werts (`USINT`) um — nur das normale VT-Objekt (`Picture`, via `Q_NumericValue`), kein AUX-Objekt.

Allgemeines Muster siehe [SwitchPic(Col)-Bausteine (gemeinsames Muster)](./SwitchPic-Bausteine.md).

![SwitchPic_5_1](SwitchPic_5_1.svg)

## Schnittstelle (QI/QO)

Der Baustein besitzt den Dateneingang `QI` (`BOOL`, Standardwert `TRUE`, Quality Input) und den Datenausgang `QO` (`BOOL`, Quality Output). `QI` wird direkt an `Q_NumericValue.QI` und `Q_NumericValue.QO` direkt an `QO` durchgereicht.

## Zusammenfassung

Variante "1" (nur normales Objekt) der 5-Zustände-Bildumschaltung — das 5-Zustände-Gegenstück zu [`SwitchPic_2_1`](./SwitchPic_2_1.md).

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

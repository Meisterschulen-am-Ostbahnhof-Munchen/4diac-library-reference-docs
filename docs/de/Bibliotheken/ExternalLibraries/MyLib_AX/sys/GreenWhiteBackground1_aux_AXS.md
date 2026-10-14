# GreenWhiteBackground1_aux_AXS

![GreenWhiteBackground1_aux_AXS_network](./GreenWhiteBackground1_aux_AXS_network.svg)

* * * * * * * * * *

## Einleitung

`GreenWhiteBackground1_aux_AXS` schaltet die VT-Hintergrundfarbe eines Objekts anhand eines booleschen Selector-Signals: `TRUE` → **Grün**, `FALSE` → **Weiß**. Das Selector-Signal kommt über einen `AX`-Adapter-Socket (`DI1`). Die Objekt-ID wird über den strukturierten Typ `s1ObjectID` (`u16ObjIds`) übergeben.

Allgemeines Muster (Selector → `AX_SEL`/`F_SEL` → `Q_BackgroundColour`) siehe [Background-Farbbausteine (gemeinsames Muster)](./Background-Farbbausteine.md).

Quality-Muster: Die SubApp besitzt den Ereigniseingang `INIT`, den Ereignisausgang `INITO`, den Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`, „Quality Input – TRUE to enable service“) und den Datenausgang `QO` (`BOOL`, „Quality Output – TRUE if service executed successfully“). Sie werden direkt an `Q_BackgroundColourAux` durchgereicht: `INIT` → `Q_BackgroundColourAux.INIT`, `Q_BackgroundColourAux.INITO` → `INITO`, `QI` → `Q_BackgroundColourAux.QI`, `Q_BackgroundColourAux.QO` → `QO`.

![GreenWhiteBackground1_aux_AXS](GreenWhiteBackground1_aux_AXS.svg)

## Zusammenfassung

Eine von vielen Varianten der Background-Farbbausteine-Familie: Farbpaar Grün/Weiß, 1 Objekt, Adapter-Selector, struct. Objekt-ID.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

# NumericValue_TO_AR2

![NumericValue_TO_AR2_network](./NumericValue_TO_AR2_network.svg)

* * * * * * * * * *

## Einleitung

`NumericValue_TO_AR2` liest ein VT-Eingabefeld (`NumericValue_PHYSA`) und reicht den Wert bidirektional als AR2-Adapter (`VALUEO`) an einen Consumer weiter - im Gegensatz zu [`INI_IN_AND_STORE_AR2`](./INI_IN_AND_STORE_AR2.md) speichert dieser Baustein selbst **nichts**: die Persistierung obliegt dem Consumer (z. B. `AR_CALIBRATE.Y_Offset`/`.Y_Scale`, oder direkt ein `INI_AR2`). Was der Consumer ueber denselben AR2-Plug zurueckechot, wird auf das VT-Feld zurueckgeschrieben - auch beim Boot, sodass Eingabefeld und tatsaechlich aktiver Wert synchron bleiben.

![NumericValue_TO_AR2](NumericValue_TO_AR2.svg)

## Technische Besonderheiten

- Reine VT-zu-AR2-Bruecke: kein Speicher-FB im Netzwerk, im Gegensatz zur INI/NVS-Familie.
- Historischer Hinweis: laut Quellcode-Kommentar aus `INI_IN_AND_STORE_AR2` umbenannt, als der Baustein von einer eigenstaendigen Speicherfunktion zu einer reinen Bruecke wurde.

## Qualitätsanschlüsse (QI/QO)

Der Baustein besitzt den Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`, Quality Input - TRUE aktiviert den Dienst) und den Datenausgang `QO` (`BOOL`, Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde). `QI` wird an `Q_NumericValue_PHYSA.QI` und `NumericValue_PHYSA.QI` weitergereicht; deren `QO`-Ausgänge werden im Baustein `AND_QO` (`iec61131::booleanOperators::AND_BOOL_2`, `Q_NumericValue_PHYSA.QO` → `IN1`, `NumericValue_PHYSA.QO` → `IN2`) zum Ausgang `QO` verknüpft.

Zusätzlich besitzt der Baustein die Ereignisse `INIT` (Eingang, `EInit`, Initialisierungsanforderung) und `INITO` (Ausgang, `EInit`, Initialisierungsbestätigung): Init-Kette: `INIT` → `Q_NumericValue_PHYSA.INIT`, `Q_NumericValue_PHYSA.INITO` → `NumericValue_PHYSA.INIT`, `NumericValue_PHYSA.INITO` → `INITO` und `AND_QO.REQ`.

## Zusammenfassung

Bidirektionale VT-Eingabefeld-zu-AR2-Bruecke ohne eigene Persistierung - der Consumer speichert, dieser Baustein zeigt nur an und leitet weiter.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

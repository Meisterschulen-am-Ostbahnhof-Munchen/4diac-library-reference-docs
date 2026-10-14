# NVS_IN_AND_STORE_AR

![NVS_IN_AND_STORE_AR_network](./NVS_IN_AND_STORE_AR_network.svg)

* * * * * * * * * *

## Einleitung

`NVS_IN_AND_STORE_AR` speichert einen über VT eingegebenen physikalisch skalierten REAL-Wert (AR-Adapter) persistent im ESP32-Flash (NVS) — sonst identisch zu [`INI_IN_AND_STORE_AR`](./INI_IN_AND_STORE_AR.md).

Allgemeines Muster siehe [INI_IN_AND_STORE / NVS_IN_AND_STORE (gemeinsames Muster)](./INI-NVS-Speicherbausteine.md).

![NVS_IN_AND_STORE_AR](NVS_IN_AND_STORE_AR.svg)

## Qualitätsanschlüsse (QI/QO)

Der Baustein besitzt den Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`, Quality Input - TRUE aktiviert den Dienst) und den Datenausgang `QO` (`BOOL`, Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde). `QI` ist mit `Q_NumericValue_PHYSA.QI` verbunden, `Q_NumericValue_PHYSA.QO` mit `QO`.

Zusätzlich besitzt der Baustein die Ereignisse `INIT` (Eingang, `EInit`, Initialisierungsanforderung) und `INITO` (Ausgang, `EInit`, Initialisierungsbestätigung): `INIT` → `Q_NumericValue_PHYSA.INIT`, `Q_NumericValue_PHYSA.INITO` → `INITO`.

## Zusammenfassung

NVS-Pendant zu `INI_IN_AND_STORE_AR`.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

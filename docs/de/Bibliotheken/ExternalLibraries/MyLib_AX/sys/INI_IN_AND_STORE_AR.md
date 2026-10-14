# INI_IN_AND_STORE_AR

![INI_IN_AND_STORE_AR_network](./INI_IN_AND_STORE_AR_network.svg)

* * * * * * * * * *

## Einleitung

`INI_IN_AND_STORE_AR` speichert einen über VT eingegebenen physikalisch skalierten REAL-Wert (AR-Adapter, `NumericValue_PHYSA`/`Q_NumericValue_PHYSA` mit `NumericObjectPool_S`-Skalierung: `r32Scale`, `i32Offset`, `u8Decimals`) persistent in einer INI-Datei.

Allgemeines Muster siehe [INI_IN_AND_STORE / NVS_IN_AND_STORE (gemeinsames Muster)](./INI-NVS-Speicherbausteine.md).

Schnittstelle (Quality-Muster): Ereignis-Eingang `INIT` und Ereignis-Ausgang `INITO` (je `EInit`), Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`, „Quality Input – TRUE to enable service“) und Datenausgang `QO` (`BOOL`, „Quality Output – TRUE if service executed successfully“). Initialisierung: `INIT` → `NumericValue_PHYSA.INIT`; `NumericValue_PHYSA.INITO` → `INI_AR.INIT`; `INI_AR.INITO` → `Q_NumericValue_PHYSA.INIT`; `Q_NumericValue_PHYSA.INITO` → `INITO` und → `AND_QO.REQ`. Qualität: `QI` → `Q_NumericValue_PHYSA.QI`, `NumericValue_PHYSA.QI`, `INI_AR.QI`; `Q_NumericValue_PHYSA.QO`, `NumericValue_PHYSA.QO`, `INI_AR.QO` → `AND_QO.IN1` bis `AND_QO.IN3` (in dieser Reihenfolge); `AND_QO.OUT` → `QO` (`AND_QO`: `iec61131::booleanOperators::AND_BOOL_3`).

![INI_IN_AND_STORE_AR](INI_IN_AND_STORE_AR.svg)

## Zusammenfassung

AR-Variante (physikalisch skaliertes REAL) der INI-Speicherfamilie.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

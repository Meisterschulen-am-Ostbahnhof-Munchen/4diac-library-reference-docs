# NVS_IN_AND_STORE_AR2

![NVS_IN_AND_STORE_AR2_network](./NVS_IN_AND_STORE_AR2_network.svg)

* * * * * * * * * *

## Einleitung

`NVS_IN_AND_STORE_AR2` ist die AR2-Adapter-Variante von `NVS_IN_AND_STORE_AR` (ESP32-Flash-Speicherung statt INI-Datei): ein per VT eingegebener physikalisch skalierter REAL-Wert wird ueber `logiBUS::storage::esp32_nvs::NVS_AR2` persistent im NVS-Flash gespeichert. Da `NVS_AR2` (anders als die INI-Variante) auch kein eigenes `SECTION` kennt, entfaellt dieser Parameter gegenueber `INI_IN_AND_STORE_AR2`. Wie dort wird `AR_TO_AR2.AR_OUT` per `AR_SPLIT_2` verdoppelt: VT-Anzeige (`Q_NumericValue_PHYSA`) + `VALUEO` fuer einen weiteren Consumer.

Allgemeines Muster siehe [INI_IN_AND_STORE / NVS_IN_AND_STORE (gemeinsames Muster)](./INI-NVS-Speicherbausteine.md).

![NVS_IN_AND_STORE_AR2](NVS_IN_AND_STORE_AR2.svg)

## Technische Besonderheiten

- `SETM=TRUE`, gleiche Begruendung wie bei [`INI_IN_AND_STORE_AR2`](./INI_IN_AND_STORE_AR2.md): Live-Aenderungen werden sofort bestaetigt, nicht erst beim naechsten Boot.
- Kein `SECTION`-Parameter, da NVS-Keys flach organisiert sind (kein INI-Abschnittskonzept).

## Qualitätsanschlüsse (QI/QO)

Der Baustein besitzt den Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`, Quality Input - TRUE aktiviert den Dienst) und den Datenausgang `QO` (`BOOL`, Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde). `QI` wird an `Q_NumericValue_PHYSA`, `NumericValue_PHYSA` und `NVS_AR2` verteilt; deren `QO`-Ausgänge werden von `AND_QO` (`iec61131::booleanOperators::AND_BOOL_3`) zu `QO` verknüpft.

Zusätzlich besitzt der Baustein die Ereignisse `INIT` (Eingang, `EInit`, Initialisierungsanforderung) und `INITO` (Ausgang, `EInit`, Initialisierungsbestätigung). Initialisierung: `INIT` → `Q_NumericValue_PHYSA.INIT`; `Q_NumericValue_PHYSA.INITO` → `NumericValue_PHYSA.INIT`; `NumericValue_PHYSA.INITO` → `NVS_AR2.INIT`; `NVS_AR2.INITO` → `INITO` und → `AND_QO.REQ`. Qualität: `QI` → `Q_NumericValue_PHYSA.QI`, `NumericValue_PHYSA.QI`, `NVS_AR2.QI`; `Q_NumericValue_PHYSA.QO`, `NumericValue_PHYSA.QO`, `NVS_AR2.QO` → `AND_QO.IN1` bis `AND_QO.IN3` (in dieser Reihenfolge); `AND_QO.OUT` → `QO` (`AND_QO`: `iec61131::booleanOperators::AND_BOOL_3`).

## Zusammenfassung

AR2-Adapter-Variante der NVS-Speicherfamilie (ESP32-Flash statt INI-Datei), mit zusaetzlichem `VALUEO`-Ausgang.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

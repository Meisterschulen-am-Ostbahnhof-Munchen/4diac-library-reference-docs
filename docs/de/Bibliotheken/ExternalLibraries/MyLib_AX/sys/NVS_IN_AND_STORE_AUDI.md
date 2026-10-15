# NVS_IN_AND_STORE_AUDI

![NVS_IN_AND_STORE_AUDI_network](./NVS_IN_AND_STORE_AUDI_network.svg)

* * * * * * * * * *

## Einleitung

`NVS_IN_AND_STORE_AUDI` speichert einen über VT eingegebenen UDINT-Wert (AUDI-Adapter) persistent im ESP32-Flash (NVS) — sonst identisch zu [`INI_IN_AND_STORE_AUDI`](./INI_IN_AND_STORE_AUDI.md).

Allgemeines Muster siehe [INI_IN_AND_STORE / NVS_IN_AND_STORE (gemeinsames Muster)](./INI-NVS-Speicherbausteine.md).

![NVS_IN_AND_STORE_AUDI](NVS_IN_AND_STORE_AUDI.svg)

## Qualitätsanschlüsse (QI/QO)

Der Baustein besitzt den Ereigniseingang `INIT` (`EInit`, Initialization Request), den Ereignisausgang `INITO` (`EInit`, Initialization Confirm), den Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`, Quality Input - TRUE aktiviert den Dienst) und den Datenausgang `QO` (`BOOL`, Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde).

- Initialisierungskette: `INIT` → `ID.INIT`, `ID.INITO` → `NVS_AUDI.INIT`, `NVS_AUDI.INITO` → `Q_NumericValue_AUDI.INIT`, `Q_NumericValue_AUDI.INITO` → `INITO` (und zusätzlich → `AND_QO.REQ`)
- `QI` ist mit `Q_NumericValue_AUDI.QI`, `NVS_AUDI.QI` und `ID.QI` verbunden
- `QO` ist die UND-Verknüpfung (`AND_QO`, Typ `iec61131::booleanOperators::AND_BOOL_3`) von `Q_NumericValue_AUDI.QO`, `NVS_AUDI.QO` und `ID.QO`

## Zusammenfassung

NVS-Pendant zu `INI_IN_AND_STORE_AUDI`.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

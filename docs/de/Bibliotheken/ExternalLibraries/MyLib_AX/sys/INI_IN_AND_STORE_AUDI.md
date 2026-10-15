# INI_IN_AND_STORE_AUDI

![INI_IN_AND_STORE_AUDI_network](./INI_IN_AND_STORE_AUDI_network.svg)

* * * * * * * * * *

## Einleitung

`INI_IN_AND_STORE_AUDI` speichert einen über VT eingegebenen UDINT-Wert (AUDI-Adapter) persistent in einer INI-Datei.

Allgemeines Muster siehe [INI_IN_AND_STORE / NVS_IN_AND_STORE (gemeinsames Muster)](./INI-NVS-Speicherbausteine.md).

![INI_IN_AND_STORE_AUDI](INI_IN_AND_STORE_AUDI.svg)

## Ereignis- und Qualitätsanschlüsse (INIT/INITO, QI/QO)

Der Baustein besitzt den Ereigniseingang `INIT` und den Ereignisausgang `INITO` (je `EInit`), den Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`, Quality Input - TRUE aktiviert den Dienst) und den Datenausgang `QO` (`BOOL`, Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde).

Die Initialisierung läuft als Kette durch die inneren Bausteine: `INIT` → `ID.INIT`, `ID.INITO` → `INI_AUDI.INIT`, `INI_AUDI.INITO` → `Q_NumericValue_AUDI.INIT`, `Q_NumericValue_AUDI.INITO` → `INITO` (und → `AND_QO.REQ`). `QI` geht an `ID`, `INI_AUDI` und `Q_NumericValue_AUDI`. `QO` ist der Ausgang der Instanz `AND_QO` (`iec61131::booleanOperators::AND_BOOL_3`), die `Q_NumericValue_AUDI.QO`, `INI_AUDI.QO` und `ID.QO` verknüpft.

## Zusammenfassung

AUDI-Variante (UDINT) der INI-Speicherfamilie.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

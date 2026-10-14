# INI_IN_AND_STORE_AUDI

![INI_IN_AND_STORE_AUDI_network](./INI_IN_AND_STORE_AUDI_network.svg)

* * * * * * * * * *

## Einleitung

`INI_IN_AND_STORE_AUDI` speichert einen über VT eingegebenen UDINT-Wert (AUDI-Adapter) persistent in einer INI-Datei.

Allgemeines Muster siehe [INI_IN_AND_STORE / NVS_IN_AND_STORE (gemeinsames Muster)](./INI-NVS-Speicherbausteine.md).

![INI_IN_AND_STORE_AUDI](INI_IN_AND_STORE_AUDI.svg)

## Qualitätsanschlüsse (QI/QO)

Der Baustein besitzt den Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`, Quality Input - TRUE aktiviert den Dienst) und den Datenausgang `QO` (`BOOL`, Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde). `QI` ist mit `Q_NumericValue_AUDI.QI` verbunden, `Q_NumericValue_AUDI.QO` mit `QO`.

Der Baustein besitzt weiterhin keine eigenen Ereignisanschlüsse.

## Zusammenfassung

AUDI-Variante (UDINT) der INI-Speicherfamilie.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

# NVS_IN_AND_STORE_UDINT

![NVS_IN_AND_STORE_UDINT_network](./NVS_IN_AND_STORE_UDINT_network.svg)

* * * * * * * * * *

## Einleitung

`NVS_IN_AND_STORE_UDINT` speichert einen über VT eingegebenen UDINT-Wert persistent im ESP32-Flash (NVS) statt in einer INI-Datei — ansonsten identisch zu [`INI_IN_AND_STORE_UDINT`](./INI_IN_AND_STORE_UDINT.md).

Allgemeines Muster siehe [INI_IN_AND_STORE / NVS_IN_AND_STORE (gemeinsames Muster)](../../MyLib_AX/sys/INI-NVS-Speicherbausteine.md).

**Quality-Anschlüsse:** Der Baustein besitzt zusätzlich den Dateneingang `QI` (BOOL, Standard `TRUE`, „Quality Input – TRUE, um den Dienst freizugeben“) und den Datenausgang `QO` (BOOL, „Quality Output – TRUE, wenn der Dienst erfolgreich ausgeführt wurde“). `QI` ist mit `Q_NumericValue.QI` und `Q_NumericValue.QO` mit `QO` verbunden.

![NVS_IN_AND_STORE_UDINT](NVS_IN_AND_STORE_UDINT.svg)

## Zusammenfassung

NVS-Pendant zu `INI_IN_AND_STORE_UDINT` (MyLib_B).

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

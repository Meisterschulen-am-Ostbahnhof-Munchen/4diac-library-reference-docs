# NVS_IN_AND_STORE_UDINT

![NVS_IN_AND_STORE_UDINT_network](./NVS_IN_AND_STORE_UDINT_network.svg)

* * * * * * * * * *

## Einleitung

`NVS_IN_AND_STORE_UDINT` speichert einen über VT eingegebenen UDINT-Wert persistent im ESP32-Flash (NVS) statt in einer INI-Datei — ansonsten identisch zu [`INI_IN_AND_STORE_UDINT`](./INI_IN_AND_STORE_UDINT.md).

Allgemeines Muster siehe [INI_IN_AND_STORE / NVS_IN_AND_STORE (gemeinsames Muster)](../../MyLib_AX/sys/INI-NVS-Speicherbausteine.md).

**Quality-Anschlüsse:** Der Baustein besitzt zusätzlich den Dateneingang `QI` (BOOL, Standard `TRUE`, „Quality Input – TRUE, um den Dienst freizugeben“) und den Datenausgang `QO` (BOOL, „Quality Output – TRUE, wenn der Dienst erfolgreich ausgeführt wurde“). `QI` wird an `Q_NumericValue.QI`, `InputNumber_I1.QI` und `NVS.QI` durchgereicht (kein fester `QI=TRUE` mehr an den internen FBs). `QO` ist das `AND` (`AND_QO`, `AND_BOOL_3`) der `QO`-Ausgänge von `Q_NumericValue` (`IN1`), `InputNumber_I1` (`IN2`) und `NVS` (`IN3`); `AND_QO.REQ` wird von `Q_NumericValue.INITO` ausgelöst (zusätzlich zu `Q_NumericValue.INITO` → `INITO`).

![NVS_IN_AND_STORE_UDINT](NVS_IN_AND_STORE_UDINT.svg)

## Zusammenfassung

NVS-Pendant zu `INI_IN_AND_STORE_UDINT` (MyLib_B).

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

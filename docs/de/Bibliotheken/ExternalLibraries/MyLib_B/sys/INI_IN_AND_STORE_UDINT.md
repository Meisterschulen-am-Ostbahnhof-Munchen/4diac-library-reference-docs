# INI_IN_AND_STORE_UDINT

![INI_IN_AND_STORE_UDINT_network](./INI_IN_AND_STORE_UDINT_network.svg)

* * * * * * * * * *

## Einleitung

`INI_IN_AND_STORE_UDINT` speichert einen über VT eingegebenen UDINT-Wert persistent in einer INI-Datei und lädt ihn beim Deployment automatisch wieder. Dies ist die MyLib_B-Variante der aus MyLib_AX bekannten INI-Speicherfamilie.

Allgemeines Muster siehe [INI_IN_AND_STORE / NVS_IN_AND_STORE (gemeinsames Muster)](../../MyLib_AX/sys/INI-NVS-Speicherbausteine.md).

**Quality-Anschlüsse:** Der Baustein besitzt zusätzlich den Dateneingang `QI` (BOOL, Standard `TRUE`, „Quality Input – TRUE, um den Dienst freizugeben“) und den Datenausgang `QO` (BOOL, „Quality Output – TRUE, wenn der Dienst erfolgreich ausgeführt wurde“). `QI` ist mit `Q_NumericValue.QI`, `InputNumber_I1.QI` und `INI.QI` verbunden; `QO` ist das UND (`AND_QO`, `iec61131::booleanOperators::AND_BOOL_3`) aus `Q_NumericValue.QO`, `InputNumber_I1.QO` und `INI.QO`.

Zusätzlich besitzt der Baustein die Ereignisse `INIT` (Eingang, `EInit`) und `INITO` (Ausgang, `EInit`); die Initialisierung wird gekettet: `INIT` → `InputNumber_I1.INIT`; `InputNumber_I1.INITO` → `INI.INIT`; `INI.INITO` → `Q_NumericValue.INIT`; `Q_NumericValue.INITO` → `INITO` (und → `AND_QO.REQ`).

![INI_IN_AND_STORE_UDINT](INI_IN_AND_STORE_UDINT.svg)

## Zusammenfassung

UDINT-Variante der INI-Speicherfamilie (MyLib_B).

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

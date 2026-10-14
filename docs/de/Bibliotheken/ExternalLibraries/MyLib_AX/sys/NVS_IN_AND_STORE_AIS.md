# NVS_IN_AND_STORE_AIS

![NVS_IN_AND_STORE_AIS_network](./NVS_IN_AND_STORE_AIS_network.svg)

* * * * * * * * * *

## Einleitung

`NVS_IN_AND_STORE_AIS` speichert einen über VT eingegebenen String-Wert (AIS-Adapter) persistent im ESP32-Flash (NVS) statt in einer INI-Datei — sonst identisch zu [`INI_IN_AND_STORE_AIS`](./INI_IN_AND_STORE_AIS.md) (kein `SECTION`-Parameter, da NVS anders organisiert ist als INI-Dateien).

Allgemeines Muster siehe [INI_IN_AND_STORE / NVS_IN_AND_STORE (gemeinsames Muster)](./INI-NVS-Speicherbausteine.md).

![NVS_IN_AND_STORE_AIS](NVS_IN_AND_STORE_AIS.svg)

## Qualitätsanschlüsse (QI/QO)

Der Baustein besitzt den Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`, Quality Input - TRUE aktiviert den Dienst) und den Datenausgang `QO` (`BOOL`, Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde). `QI` geht an `Q_StringValue_AIS`, `NVS_AIS` und `StringValue_AIS`. `QO` ist der Ausgang der Instanz `AND_QO` (`iec61131::booleanOperators::AND_BOOL_3`), die `Q_StringValue_AIS.QO`, `NVS_AIS.QO` und `StringValue_AIS.QO` verknüpft.

Zusätzlich besitzt der Baustein die Ereignisse `INIT` (Eingang, `EInit`, Initialisierungsanforderung) und `INITO` (Ausgang, `EInit`, Initialisierungsbestätigung): `INIT` → `Q_StringValue_AIS.INIT`, `Q_StringValue_AIS.INITO` → `NVS_AIS.INIT`, `NVS_AIS.INITO` → `StringValue_AIS.INIT`, `StringValue_AIS.INITO` → `INITO` (und → `AND_QO.REQ`).

## Zusammenfassung

NVS-Pendant zu `INI_IN_AND_STORE_AIS`.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

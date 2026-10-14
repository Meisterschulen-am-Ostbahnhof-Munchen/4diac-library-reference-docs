# NVS_IN_AND_STORE_AIS

![NVS_IN_AND_STORE_AIS_network](./NVS_IN_AND_STORE_AIS_network.svg)

* * * * * * * * * *

## Einleitung

`NVS_IN_AND_STORE_AIS` speichert einen über VT eingegebenen String-Wert (AIS-Adapter) persistent im ESP32-Flash (NVS) statt in einer INI-Datei — sonst identisch zu [`INI_IN_AND_STORE_AIS`](./INI_IN_AND_STORE_AIS.md) (kein `SECTION`-Parameter, da NVS anders organisiert ist als INI-Dateien).

Allgemeines Muster siehe [INI_IN_AND_STORE / NVS_IN_AND_STORE (gemeinsames Muster)](./INI-NVS-Speicherbausteine.md).

![NVS_IN_AND_STORE_AIS](NVS_IN_AND_STORE_AIS.svg)

## Qualitätsanschlüsse (QI/QO)

Der Baustein besitzt den Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`, Quality Input - TRUE aktiviert den Dienst) und den Datenausgang `QO` (`BOOL`, Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde). `QI` ist mit `Q_StringValue_AIS.QI` verbunden, `Q_StringValue_AIS.QO` mit `QO`.

Zusätzlich besitzt der Baustein die Ereignisse `INIT` (Eingang, `EInit`, Initialisierungsanforderung) und `INITO` (Ausgang, `EInit`, Initialisierungsbestätigung): `INIT` → `Q_StringValue_AIS.INIT`, `Q_StringValue_AIS.INITO` → `INITO`.

## Zusammenfassung

NVS-Pendant zu `INI_IN_AND_STORE_AIS`.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

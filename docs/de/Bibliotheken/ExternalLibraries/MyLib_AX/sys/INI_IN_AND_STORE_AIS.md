# INI_IN_AND_STORE_AIS

![INI_IN_AND_STORE_AIS_network](./INI_IN_AND_STORE_AIS_network.svg)

* * * * * * * * * *

## Einleitung

`INI_IN_AND_STORE_AIS` speichert einen über VT eingegebenen String-Wert (AIS-Adapter) persistent in einer INI-Datei und lädt ihn beim Deployment automatisch wieder.

Allgemeines Muster siehe [INI_IN_AND_STORE / NVS_IN_AND_STORE (gemeinsames Muster)](./INI-NVS-Speicherbausteine.md).

Schnittstelle (Quality-Muster): Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`, „Quality Input – TRUE to enable service“) und Datenausgang `QO` (`BOOL`, „Quality Output – TRUE if service executed successfully“) sind direkt mit dem VT-Schreibbaustein verbunden: `QI` → `Q_StringValue_AIS.QI`, `Q_StringValue_AIS.QO` → `QO`.

![INI_IN_AND_STORE_AIS](INI_IN_AND_STORE_AIS.svg)

## Zusammenfassung

AIS-Variante (String) der INI-Speicherfamilie.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

* [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

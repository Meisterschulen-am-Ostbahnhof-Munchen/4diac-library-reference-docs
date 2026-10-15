# INI_IN_AND_STORE_AR2_TO_REAL

![INI_IN_AND_STORE_AR2_TO_REAL_network](./INI_IN_AND_STORE_AR2_TO_REAL_network.svg)

* * * * * * * * * *

## Einleitung

`INI_IN_AND_STORE_AR2_TO_REAL` lädt und speichert einen Wert in der INI-Datei (wie [`INI_IN_AND_STORE_AR2`](INI_IN_AND_STORE_AR2.md)) und konvertiert ihn zusätzlich per `AR_R_TO_REAL` auf eine einfache REAL-Ausgangsvariable. Das wird für Eingangsvariablen von Ziel-Bausteinen benötigt, die einen echten REAL-Parameter statt eines AR-Adapter-Sockets erwarten (z. B. `CTRL_PID_AR.KP`/`TN`/`TV`).

![INI_IN_AND_STORE_AR2_TO_REAL](INI_IN_AND_STORE_AR2_TO_REAL.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung; wird an `INI.INIT` weitergereicht

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung; kommt von `INI.INITO`
- **CNF** (Confirmation): feuert, wenn ein (neuer oder initial geladener) Wert vorliegt. An einen `REQ`-Eingang eines Ziel-FB anschließen (z. B. `CTRL_PID_AR.REQ`), damit dieser den frischen Wert sofort übernimmt, statt auf den nächsten eigenen Adapter-Trigger zu warten

### **Daten-Eingänge**

- **QI** (BOOL, Initialwert `TRUE`): Quality Input, schaltet den Dienst frei; wird an `INI.QI` weitergereicht
- **KEY** (STRING): Schlüsselname in der INI-Datei
- **SECTION** (STRING): Sektionsname in der INI-Datei
- **stObj** (`logiBUS::utils::conversion::phys::NumericObjectPool_S`): Object-Pool-Eigenschaften der VT-Zahlenvariable (Default: `u16ObjId := ID_NULL, r32Scale := 1.0, i32Offset := 0, u8Decimals := 0`)

### **Daten-Ausgänge**

- **QO** (BOOL): Quality Output; kommt von `INI.QO`
- **VALUE** (REAL): persistierter/geladener Wert als einfaches REAL (kein AR-Adapter)

### **Adapter**

Es werden keine Adapter verwendet.

## Funktionsweise

Das Netzwerk enthält zwei Instanzen:

- `INI` ([`MyLib::sys::INI_IN_AND_STORE_AR2`](INI_IN_AND_STORE_AR2.md)): `KEY` → `INI.KEY`, `SECTION` → `INI.SECTION`, `stObj` → `INI.stObj`. Der Wert steht an `INI.VALUEO` (AR) bereit.
- `AR_R_TO_REAL_1` (`adapter::conversion::unidirectional::AR_R_TO_REAL`): `INI.VALUEO` → `AR_R_TO_REAL_1.AR_IN`; `AR_R_TO_REAL_1.IN` → `VALUE` und `AR_R_TO_REAL_1.CNF` → `CNF`.

Initialisierung und Qualität werden direkt durchgereicht: `INIT` → `INI.INIT`, `INI.INITO` → `INITO`, `QI` → `INI.QI` und `INI.QO` → `QO`.

## Technische Besonderheiten

- Herausgelöst aus `Aufnahme_Automatik.SUB`, wo diese Kombination dreimal identisch (KP/TN/TV) verdrahtet war
- `CNF` entsteht erst nach der AR-zu-REAL-Konvertierung und signalisiert damit, dass `VALUE` aktuell ist

## Anwendungsszenarien

- Persistierte Reglerparameter (z. B. KP/TN/TV eines PID-Reglers), die an REAL-Eingänge von Ziel-FBs angeschlossen werden

## Fazit

`INI_IN_AND_STORE_AR2_TO_REAL` kombiniert INI-Persistenz mit einer AR-zu-REAL-Wandlung und liefert den Wert samt `CNF`-Ereignis an einer einfachen REAL-Ausgangsvariable.

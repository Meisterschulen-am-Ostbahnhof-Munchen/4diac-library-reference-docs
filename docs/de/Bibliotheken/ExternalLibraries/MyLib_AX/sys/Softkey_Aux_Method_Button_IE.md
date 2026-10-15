# Softkey_Aux_Method_Button_IE

![Softkey_Aux_Method_Button_IE_network](./Softkey_Aux_Method_Button_IE_network.svg)

* * * * * * * * * *

## Einleitung

`Softkey_Aux_Method_Button_IE` erweitert [`Softkey_Aux_Method_IE`](./Softkey_Aux_Method_IE.md) (SoftKey ODER AUX ODER lokale OPC-UA-Methode) um einen **weiteren Button** (`Button_IE`, eigener `CButton` – z. B. ein IO-Diag-Testbutton, kann aber auch ein regulärer Button sein). Aktiviert eine dieser Quellen, entsteht ein gemeinsames `IND`-Ereignis.

Der Baustein dient für Ausgänge, die sowohl echt bedient (SoftKey/AUX/Web/Dashboard) als auch per zusätzlichem Button gleichberechtigt getriggert werden sollen. Der Hintergrund des Buttons wird vom Aufrufer separat aus dem resultierenden Zustand gespeist; dafür bleibt `u16ObjIdB` beim Aufrufer verfügbar.

![Softkey_Aux_Method_Button_IE](Softkey_Aux_Method_Button_IE.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: Softkey_Aux_Method_Button_IE

- **Typ**: SubAppType
- **Verwendete interne Bausteine**:
    - **SoftkeyAuxMethod**: SubApp [`MyLib::sys::Softkey_Aux_Method_IE`](./Softkey_Aux_Method_IE.md) – SoftKey, AUX oder lokale Methode
    - **Button_IE_B**: `isobus::UT::io::Button::Button_IE` (`InputEvent=BT_RELEASED_UNLATCHED`) – weiterer Button
    - **MERGE**: `iec61499::events::E_MERGE_2` – führt beide `IND`-Ereignisse zusammen
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_2` – verknüpft die `QO` von `Button_IE_B` und `SoftkeyAuxMethod`
- **Importe**: `ID_NULL`, `BT_RELEASED_UNLATCHED`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung; geht an `Button_IE_B.INIT`

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierung abgeschlossen; kommt von `SoftkeyAuxMethod.INITO`
- **IND**: Feuert, wenn SoftKey, AUX, die lokale Methode oder der zusätzliche Button aktiviert bzw. aufgerufen wird

### **Daten-Eingänge**

- **QI** (`BOOL`, Vorgabe `TRUE`): Quality Input - TRUE aktiviert den Dienst; geht an `Button_IE_B` und `SoftkeyAuxMethod`
- **u16ObjId** (`UINT`): Object-ID des SoftKeys
- **u16ObjIdA** (`UINT`): Object-ID der `AuxFunction2`
- **u16ObjIdB** (`UINT`, Vorgabe `ID_NULL`): Object-ID eines weiteren Buttons (z. B. IO-Diag-Testbutton auf der Ausgänge-Diagnoseseite; kann auch ein regulärer Button sein), gleichberechtigt zu SoftKey/AUX/Methode
- **ID_METHOD** (`WSTRING`): Lokale Methodenadresse (`ACTION=CREATE_METHOD`) für den argumentlosen Trigger – wird vom Web-Client (`vt-ui-mirror`) und direkt vom OPC-Dashboard aufgerufen

### **Daten-Ausgänge**

- **QO** (`BOOL`): Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde (AND der `QO` von `Button_IE_B` und `SoftkeyAuxMethod`)

## Programmablauf und Verbindungen

- `u16ObjId`, `u16ObjIdA`, `ID_METHOD` → `SoftkeyAuxMethod`
- `u16ObjIdB` → `Button_IE_B.u16ObjId`
- `SoftkeyAuxMethod.IND` → `MERGE.EI1`; `Button_IE_B.IND` → `MERGE.EI2`
- `MERGE.EO` → `IND`
- Initialisierung: `INIT` → `Button_IE_B.INIT`, `Button_IE_B.INITO` → `SoftkeyAuxMethod.INIT`, `SoftkeyAuxMethod.INITO` → `INITO` und → `AND_QO.REQ`
- Qualität: `QI` → `Button_IE_B.QI` und `SoftkeyAuxMethod.QI`; `Button_IE_B.QO` → `AND_QO.IN1`, `SoftkeyAuxMethod.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`

## Technische Besonderheiten

- Version 1.1 (2026-09-15): `u16ObjId_TEST` in `u16ObjIdB` umbenannt (`Button_IE_TEST` → `Button_IE_B`), da der zusätzliche Button nicht immer ein IO-Diag-Testbutton ist
- Wird u. a. von `AX_SoftkeyToggle_RS_TO_AX` für den Toggle-Zweig genutzt

## Anwendungsszenarien

- Toggle-Ausgang, der per SoftKey, Joystick, Web/Dashboard und zusätzlichem Diagnose-Button gleichberechtigt bedient werden soll

## Zusammenfassung

`Softkey_Aux_Method_Button_IE` ergänzt `Softkey_Aux_Method_IE` um einen weiteren Button und liefert ein gemeinsames `IND`-Ereignis.

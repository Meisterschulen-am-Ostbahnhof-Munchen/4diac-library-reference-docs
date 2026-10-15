# AX_SoftkeyToggle_RS_TO_QXA_BG

![AX_SoftkeyToggle_RS_TO_QXA_BG_network](./AX_SoftkeyToggle_RS_TO_QXA_BG_network.svg)

* * * * * * * * * *

## Einleitung

`AX_SoftkeyToggle_RS_TO_QXA_BG` kapselt [`AX_SoftkeyToggle_RS_TO_AX`](./AX_SoftkeyToggle_RS_TO_AX.md) (Toggle-/Reset-Zustand) und treibt daraus **einen physischen Ausgang**, die **Hintergrundfarbe** und einen **lokalen State-Publish**:

- Toggle: SoftKey **oder** AUX **oder** lokale Web-/Dashboard-Methode **oder** IO-Diag-Testbutton
- Reset: SoftKey **oder** AUX **oder** Methode
- Hintergrundfarbe: SoftKey, AUX-Handle und IO-Diag-Testbutton gemeinsam aus einer `GreenWhiteBackground4_AX`
- Publish: Zustand für Web-Client/OPC-Dashboard-Hintergrundfarbe

Generisch für jeden Ein/Aus-Ausgang, der von einer globalen Sammel-AUS-Taste mit zurückgesetzt werden muss (z. B. Sägeblattantrieb).

![AX_SoftkeyToggle_RS_TO_QXA_BG](AX_SoftkeyToggle_RS_TO_QXA_BG.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: AX_SoftkeyToggle_RS_TO_QXA_BG

- **Typ**: SubAppType
- **Verwendete interne Bausteine**:
    - **ToggleState**: SubApp [`MyLib::sys::AX_SoftkeyToggle_RS_TO_AX`](./AX_SoftkeyToggle_RS_TO_AX.md) – Toggle-/Reset-Zustand (inkl. `FORCE_OFF`)
    - **AX_SPLIT_3**: `adapter::events::unidirectional::AX_SPLIT_3` – verteilt den Zustand auf drei Verbraucher
    - **logiBUS_QXA**: `logiBUS::io::DQ::logiBUS_QXA` (`QI` vom Baustein-Eingang `QI`) – physischer Ausgang
    - **GreenWhiteBackground_AX**: SubApp [`MyLib::sys::GreenWhiteBackground4_AX`](./GreenWhiteBackground4_AX.md) – Hintergrundfarbe für SoftKey, AUX und Testbutton
    - **PUBLISH_STATE**: `adapter::net::AX_PUBLISH_1` (`QI` vom Baustein-Eingang `QI`) – lokaler State-Publish
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_4` – verknüpft die `QO`-Ausgänge von `logiBUS_QXA`, `PUBLISH_STATE`, `GreenWhiteBackground_AX` und `ToggleState` per UND zum Baustein-Ausgang `QO`
- **Importe**: `ID_NULL`, `logiBUS_DO`, `logiBUS_DO_S`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialization Request

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialization Confirm

Die Signalverarbeitung (Toggle/Reset) läuft weiterhin über die Adapter.

### **Daten-Eingänge**

- **QI** (`BOOL`, Vorgabe `TRUE`): Quality Input – TRUE, um den Dienst freizugeben
- **u16ObjId** (`UINT`, Vorgabe `ID_NULL`): Object-ID Softkey Ein/Aus (Toggle), trägt auch die Hintergrundfarbe
- **u16ObjIdA** (`UINT`, Vorgabe `ID_NULL`): Object-ID `AuxFunction2` (Joystick) für den Toggle – gleichberechtigt zum SoftKey, zeigt die Hintergrundfarbe am Aux-Handle mit
- **u16ObjIdB** (`UINT`, Vorgabe `ID_NULL`): Object-ID IO-Diag-Testbutton (Ausgänge-Diagnoseseite) – toggelt gleichberechtigt mit, zeigt die Hintergrundfarbe gemeinsam mit SoftKey und AUX
- **u16ObjId_RESET** (`UINT`, Vorgabe `ID_NULL`): Object-ID Softkey „Alles Aus" (Reset, hart auf AUS), z. B. `SoftKey_AUS`
- **u16ObjIdA_RESET** (`UINT`, Vorgabe `ID_NULL`): Object-ID `AuxFunction2` (Joystick) für den Reset – gleichberechtigt zum Softkey „Alles Aus"
- **Output** (`logiBUS::io::DQ::logiBUS_DO_S`, Vorgabe `logiBUS_DO::Invalid`): Ausgang `Output_Q1..Q12`
- **ID_TOGGLE_METHOD** (`WSTRING`): Lokale Methodenadresse (`ACTION=CREATE_METHOD`) für den argumentlosen Toggle-Trigger – wird vom Web-Client (`vt-ui-mirror`) und direkt vom OPC-Dashboard aufgerufen
- **ID_RESET_METHOD** (`WSTRING`): Lokale Methodenadresse (`ACTION=CREATE_METHOD`) für den argumentlosen Reset-Trigger (hart auf AUS) – vom Web-Client und OPC-Dashboard aufgerufen
- **ID_STATE_WRITE** (`WSTRING`): Lokale Publish-Adresse (`ACTION=WRITE`) für den tatsächlichen Toggle-Zustand (`AX_T_FF_SR.Q`) – vom Web-Client/Dashboard remote abonniert

### **Daten-Ausgänge**

- **QO** (`BOOL`): Quality Output – TRUE, wenn der Dienst erfolgreich ausgeführt wurde (UND der `QO` der inneren Bausteine)

### **Adapter**

- **FORCE_OFF** (Socket, `adapter::types::unidirectional::AX`): Optional: steigende Flanke resettet hart auf AUS (z. B. Sicherheitsabschaltung) – reiner Durchreich-Socket an `ToggleState.FORCE_OFF`. Unverdrahtet lassen (fester FALSE-Literal), wenn nicht gebraucht.

## Programmablauf und Verbindungen

- `u16ObjId`, `u16ObjIdA`, `u16ObjIdB`, `u16ObjId_RESET`, `u16ObjIdA_RESET`, `ID_TOGGLE_METHOD`, `ID_RESET_METHOD` → gleichnamige Eingänge von `ToggleState`
- `FORCE_OFF` → `ToggleState.FORCE_OFF`
- `ToggleState.OUT` → `AX_SPLIT_3.IN`
- `AX_SPLIT_3.OUT1` → `logiBUS_QXA.OUT` (`Output` → `logiBUS_QXA.Output`)
- `AX_SPLIT_3.OUT2` → `GreenWhiteBackground_AX.DI1` (`u16ObjId`, `u16ObjIdA`, `u16ObjIdB` → gleichnamige Eingänge)
- `AX_SPLIT_3.OUT3` → `PUBLISH_STATE.IN` (`ID_STATE_WRITE` → `PUBLISH_STATE.ID`)
- Initialisierungskette: `INIT` → `logiBUS_QXA.INIT`, `logiBUS_QXA.INITO` → `PUBLISH_STATE.INIT`, `PUBLISH_STATE.INITO` → `GreenWhiteBackground_AX.INIT`, `GreenWhiteBackground_AX.INITO` → `ToggleState.INIT`, `ToggleState.INITO` → `INITO`; zusätzlich `ToggleState.INITO` → `AND_QO.REQ`
- `QI` → `logiBUS_QXA.QI`, `PUBLISH_STATE.QI`, `GreenWhiteBackground_AX.QI` und `ToggleState.QI`
- `logiBUS_QXA.QO` → `AND_QO.IN1`, `PUBLISH_STATE.QO` → `AND_QO.IN2`, `GreenWhiteBackground_AX.QO` → `AND_QO.IN3`, `ToggleState.QO` → `AND_QO.IN4`; `AND_QO.OUT` → `QO`

## Technische Besonderheiten

- Seit Version 7.0 nur noch Wrapper: Toggle, Reset und `AX_T_FF_SR` stecken in `AX_SoftkeyToggle_RS_TO_AX`, das auch für Mehrkanal-Sequenzierung (mehrere zeitversetzte Ausgänge) verwendet wird
- Seit Version 6.0: ein `GreenWhiteBackground4_AX` statt zweier `GreenWhiteBackground1_AX`, damit auch das Joystick-Handle eine Rückmeldung bekommt
- Version 8.0 (2026-09-15): `FORCE_OFF`-Socket ergänzt (rein additiv; bestehende Instanzen müssen ihn verdrahten, z. B. auf einen festen FALSE-Literal)
- Historie: 1.0 Reset-Set-Toggle-Variante von `AX_SoftkeyToggle_TO_QXA_BG`; 2.0 IO-Diag-Testbutton, zwei lokale RPC-Methoden und State-Publish; 3.0 AUX ergänzt; 4.0/5.0 Auslagerung in `Softkey_Aux_Method_IE` bzw. `Softkey_Aux_Method_Button_IE`

## Anwendungsszenarien

- Ein/Aus-Ausgang mit SoftKey, Joystick, IO-Diag-Testbutton und Web-/Dashboard-Bedienung sowie einer globalen „Alles Aus"-Taste (z. B. Sägeblattantrieb)

## Zusammenfassung

`AX_SoftkeyToggle_RS_TO_QXA_BG` verbindet den Toggle-/Reset-Zustand aus `AX_SoftkeyToggle_RS_TO_AX` mit physischem Ausgang, Hintergrundfarbe und lokalem State-Publish.

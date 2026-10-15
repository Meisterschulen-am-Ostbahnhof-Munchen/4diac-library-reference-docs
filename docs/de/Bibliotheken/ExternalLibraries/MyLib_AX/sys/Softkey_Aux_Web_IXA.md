# Softkey_Aux_Web_IXA

![Softkey_Aux_Web_IXA_network](./Softkey_Aux_Web_IXA_network.svg)

* * * * * * * * * *

## Einleitung

`Softkey_Aux_Web_IXA` führt einen VT-SoftKey, eine AUX-Zuweisung (Joystick) und einen lokalen Web-Override (z. B. `vt-ui-mirror` auf demselben Modul) zu **einem gemeinsamen, kontinuierlichen AX-Pegel** zusammen – ohne Remote-Write und ohne Ereignis. Er ist für den Tastbetrieb (Jog/Tippen) gedacht, bei dem der Pegel selbst (nicht eine Flanke) weiterverarbeitet werden soll, z. B. als `G`-Eingang eines `AX_AX_SEL_AR` oder als `DI1` einer GreenWhiteBackground-Rückmeldung. Er entspricht der Kommandohälfte von `Softkey_Aux_IXA_TO_Remote_WRITE`, liefert aber `OUT` statt eines Remote-Writes.

![Softkey_Aux_Web_IXA](Softkey_Aux_Web_IXA.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung

Außer `INITO` gibt es keine Ereignis-Ausgänge – Nutzereignisse laufen über den Adapter-Plug.

### **Daten-Eingänge**

- **QI** (`BOOL`, Vorgabe `TRUE`): Quality Input – TRUE gibt den Dienst frei
- **u16ObjId** (`UINT`, Vorgabe `ID_NULL`): Object-ID des SoftKeys (VT)
- **u16ObjIdA** (`UINT`, Vorgabe `ID_NULL`): Object-ID der AuxFunction2 (VT)
- **ID** (`WSTRING`): Lokale Subscribe-Adresse für einen Web-Client (z. B. `vt-ui-mirror`) auf demselben Modul, ODER-verknüpft mit SoftKey und AUX

### **Daten-Ausgänge**

- **QO** (`BOOL`): Quality Output – UND aus `Softkey_IXA.QO`, `Aux_IXA.QO` und `SUBSCRIBE_WEB.QO`

### **Adapter**

- **OUT** (Plug, `adapter::types::unidirectional::AX`): SoftKey ODER Aux ODER Web-Override, kontinuierlicher Pegel (`TRUE`, solange gehalten/aktiv)

## Verwendete Funktionsbausteine (FBs)

- **Softkey_IXA**: `isobus::UT::io::Softkey::Softkey_IXA`
- **Aux_IXA**: `isobus::UT::io::Auxiliary::IN::Aux_IXA`
- **SUBSCRIBE_WEB**: `adapter::net::AX_SUBSCRIBE_1`
- **OR_MERGE**: `adapter::booleanOperators::AX_OR_3`
- **AND_QO**: `iec61131::booleanOperators::AND_BOOL_3`

## Programmablauf und Verbindungen

- `u16ObjId` → `Softkey_IXA.u16ObjId`; `u16ObjIdA` → `Aux_IXA.u16ObjId`; `ID` → `SUBSCRIBE_WEB.ID`
- `Softkey_IXA.IN` → `OR_MERGE.IN1`
- `Aux_IXA.IN` → `OR_MERGE.IN2`
- `SUBSCRIBE_WEB.OUT` → `OR_MERGE.IN3`
- `OR_MERGE.OUT` → `OUT`
- `INIT` → `Softkey_IXA.INIT`; `Softkey_IXA.INITO` → `Aux_IXA.INIT`; `Aux_IXA.INITO` → `SUBSCRIBE_WEB.INIT`; `SUBSCRIBE_WEB.INITO` → `INITO` (und → `AND_QO.REQ`)
- `QI` → `Softkey_IXA.QI`, `Aux_IXA.QI` und `SUBSCRIBE_WEB.QI`
- `Softkey_IXA.QO` → `AND_QO.IN1`; `Aux_IXA.QO` → `AND_QO.IN2`; `SUBSCRIBE_WEB.QO` → `AND_QO.IN3`; `AND_QO.OUT` → `QO`

## Technische Besonderheiten

- Kontinuierlicher Pegel statt Flanke: `OUT` bleibt `TRUE`, solange mindestens eine Quelle aktiv ist.
- Kein Remote-Write und kein Nutzereignis-Ausgang (nur `INITO`).

## Anwendungsszenarien

- Tastbetrieb (Jog/Tippen), bei dem der Pegel an einen Selektor oder eine Hintergrundfarben-Rückmeldung gehen soll

## Fazit

`Softkey_Aux_Web_IXA` verodert SoftKey, Aux und Web-Override zu einem AX-Pegel.

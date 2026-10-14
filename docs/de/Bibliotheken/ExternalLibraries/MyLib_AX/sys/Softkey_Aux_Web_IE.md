# Softkey_Aux_Web_IE

![Softkey_Aux_Web_IE_network](./Softkey_Aux_Web_IE_network.svg)

* * * * * * * * * *

## Einleitung

`Softkey_Aux_Web_IE` löst ein gemeinsames `IND`-Ereignis aus, wenn **SoftKey ODER AUX (Joystick) ODER Web-Remote** aktiviert wird. Der Web-Kanal ist ein extern zugeführter `BOOL`-Pegel (`AX`), der per `AX_R_TRIG` in eine Flanke gewandelt wird. Der Baustein baut auf `Softkey_Aux_IE` auf (Touchscreen + Joystick, diskreter Event-Kanal) und merged zusätzlich den Web-Kanal – ein Muster für „egal von welcher Quelle aus aktiviert, ein Ergebnis-Ereignis", z. B. für die Override-Erkennung.

![Softkey_Aux_Web_IE](Softkey_Aux_Web_IE.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: Softkey_Aux_Web_IE

- **Typ**: SubAppType
- **Verwendete interne Bausteine**:
    - **SoftkeyAux**: SubApp `MyLib::sys::Softkey_Aux_IE` – SoftKey oder AUX
    - **WEB_R_TRIG**: `adapter::events::unidirectional::AX_R_TRIG` – wandelt den Web-Pegel in eine steigende Flanke (`EO`)
    - **MERGE**: `iec61499::events::E_MERGE_2` – führt beide Ereignisse zusammen

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung; geht an `SoftkeyAux.INIT`

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierung abgeschlossen; kommt von `SoftkeyAux.INITO`
- **IND**: Feuert, wenn SoftKey, Aux oder Web-Remote aktiviert wird

### **Daten-Eingänge**

- **QI** (`BOOL`, Vorgabe `TRUE`): Quality Input - TRUE aktiviert den Dienst; geht an `SoftkeyAux.QI`
- **u16ObjId** (`UINT`): Object-ID des SoftKeys
- **u16ObjIdA** (`UINT`): Object-ID der `AuxFunction2`

### **Daten-Ausgänge**

- **QO** (`BOOL`): Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde; kommt von `SoftkeyAux.QO`

### **Adapter**

- **WEB_IN** (Socket, `adapter::types::unidirectional::AX`): Web-Remote-Pegel von außen (z. B. `Softkey_Aux_Auto_IXA_TO_Remote_WRITE.WEB_STATE`) statt eines eigenen lokalen OPC-UA-Self-Subscribe

## Programmablauf und Verbindungen

- `u16ObjId` → `SoftkeyAux.u16ObjId`; `u16ObjIdA` → `SoftkeyAux.u16ObjIdA`
- `SoftkeyAux.IND` → `MERGE.EI1`
- `WEB_IN` → `WEB_R_TRIG.QI`; `WEB_R_TRIG.EO` → `MERGE.EI2`
- `MERGE.EO` → `IND`
- Initialisierung: `INIT` → `SoftkeyAux.INIT`; `SoftkeyAux.INITO` → `INITO`
- Qualität: `QI` → `SoftkeyAux.QI`; `SoftkeyAux.QO` → `QO`

## Technische Besonderheiten

- Version 2.0 (2026-09-16): ID-Parameter und interner Self-Subscribe (`SUBSCRIBE_R_TRIG`) entfernt, ersetzt durch den Socket `WEB_IN` plus `AX_R_TRIG`. Das behebt den Doppel-Self-Subscribe (FORTE: „Cannot add another one"), da der Web-Knoten bereits von der Kommando-Seite (`Softkey_Aux_Auto_IXA_TO_Remote_WRITE.WEB_STATE`) abonniert wird
- Es wird nur die steigende Flanke des Web-Pegels ausgewertet
- Version 1.0 (2026-09-11): initiale Implementierung

## Anwendungsszenarien

- Override-Erkennung: Ereignis, sobald der Bediener über SoftKey, Joystick oder Web eingreift
- Zusammenarbeit mit [`Softkey_Aux_Auto_IXA_TO_Remote_WRITE`](./Softkey_Aux_Auto_IXA_TO_Remote_WRITE.md) (`WEB_STATE`)

## Zusammenfassung

`Softkey_Aux_Web_IE` bündelt SoftKey, AUX und einen extern zugeführten Web-Pegel zu einem einzigen `IND`-Ereignis.

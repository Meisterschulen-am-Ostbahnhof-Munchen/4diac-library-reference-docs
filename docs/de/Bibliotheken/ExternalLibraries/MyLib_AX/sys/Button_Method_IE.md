# Button_Method_IE

![Button_Method_IE_network](./Button_Method_IE_network.svg)

* * * * * * * * * *

## Einleitung

`Button_Method_IE` löst ein gemeinsames `IND`-Ereignis aus, wenn entweder ein **VT-Button** (`Button_IE`) aktiviert oder eine **lokale argumentlose OPC-UA-Methode** (`ACTION=CREATE_METHOD`, `SERVER_0`) aufgerufen wird, z. B. vom Web-Client/Dashboard oder per `CLIENT_0` von einer anderen Station. Der Baustein ist analog zu [`Softkey_Aux_Method_IE`](./Softkey_Aux_Method_IE.md), aber für einen regulären VT-Button statt SoftKey/AUX. Er wurde aus `logiBUS_AI_Calibrate_IDA_OPC` herausgelöst, wo die Kombination (CO/CS) zweimal identisch vorkam.

![Button_Method_IE](Button_Method_IE.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

- **IND**: feuert, wenn der Button **oder** die lokale Methode aktiviert bzw. aufgerufen wird

### **Daten-Eingänge**

- **u16ObjId** (`UINT`): Object ID des Buttons
- **ID_METHOD** (`WSTRING`): lokale Methodenadresse (`ACTION=CREATE_METHOD`) für den argumentlosen Trigger; wird vom Web-Client/Dashboard und per `CLIENT_0` von einer anderen Station aufgerufen

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Verwendete Funktionsbausteine (FBs)

- **`Button`** (`isobus::UT::io::Button::Button_IE`): Parameter `QI = TRUE`, `InputEvent = BT_PRESSED_LATCHED`
- **`METHOD_SERVER`** (`iec61499::net::SERVER_0`): Parameter `QI = TRUE`, stellt die lokale Methode bereit
- **`MERGE`** (`iec61499::events::E_MERGE_2`): führt beide Auslöser zusammen

## Funktionsweise

- `u16ObjId` → `Button.u16ObjId`; `ID_METHOD` → `METHOD_SERVER.ID`
- `Button.IND` → `MERGE.EI1`
- `METHOD_SERVER.IND` → `MERGE.EI2`
- `METHOD_SERVER.IND` → `METHOD_SERVER.RSP` (sofortige Antwort)
- `MERGE.EO` → `IND`

## Technische Besonderheiten

- **`RSP` muss auf `IND` zurückverdrahtet werden.** Der Trigger ist argumentlos und braucht keine Verarbeitung. Ohne Rückverdrahtung blockiert `onServerMethodCall` in FORTE den OPC-UA-Server-Thread bis zu 4 s und hält dabei den globalen `serviceMutex` von open62541, den jeder `AX_PUBLISH_1` auf einem anderen FORTE-Thread ebenfalls benötigt (siehe `Ventilsteuerung/OPC_UA_VERNETZUNG.md`, Punkt 12).
- `BT_PRESSED_LATCHED` als Button-Ereignis (rastendes Drücken/Loslassen)

## Anwendungsszenarien

- Button, der zusätzlich per Web-Client, OPC-Dashboard oder von einer anderen Station ausgelöst werden soll (z. B. Kalibrier-Trigger)

## Zusammenfassung

`Button_Method_IE` vereint VT-Button und lokale OPC-UA-Methode zu einem gemeinsamen `IND`-Ereignis.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

# Softkey_Aux_Method_IE

![Softkey_Aux_Method_IE_network](./Softkey_Aux_Method_IE_network.svg)

* * * * * * * * * *

## Einleitung

`Softkey_Aux_Method_IE` führt drei Auslöser zu **einem gemeinsamen `IND`-Ereignis** zusammen: einen VT-SoftKey, eine AUX-Funktion (Joystick, über `Softkey_Aux_IE`) oder eine **lokale argumentlose OPC-UA-Methode** (`ACTION=CREATE_METHOD`, `SERVER_0`), die z. B. vom Web-Client (`vt-ui-mirror`) oder vom OPC-Dashboard aufgerufen wird. Der Baustein baut auf `Softkey_Aux_IE` auf und ergänzt den Methodenkanal. Im Unterschied zu `Softkey_Aux_Web_IE` (Web per `SUBSCRIBE_R_TRIG` auf eine geschriebene Variable) erfolgt der Web-/Dashboard-Auslöser hier per echtem RPC-Methodenaufruf, ohne gemeinsame Variable.

![Softkey_Aux_Method_IE](Softkey_Aux_Method_IE.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

*Keine direkten Ereignis-Eingänge vorhanden*

### **Ereignis-Ausgänge**

- **IND**: Feuert, wenn SoftKey, AUX oder die lokale Methode aktiviert bzw. aufgerufen wird

### **Daten-Eingänge**

- **u16ObjId** (`UINT`): Object-ID des SoftKeys
- **u16ObjIdA** (`UINT`): Object-ID der AuxFunction2
- **ID_METHOD** (`WSTRING`): Lokale Methodenadresse (`ACTION=CREATE_METHOD`) für den argumentlosen Trigger; wird vom Web-Client (`vt-ui-mirror`) und direkt vom OPC-Dashboard aufgerufen

### **Daten-Ausgänge**

*Keine Daten-Ausgänge vorhanden*

### **Adapter**

Dieser Funktionsblock verwendet keine Adapter.

## Verwendete Funktionsbausteine (FBs)

- **SoftkeyAux**: SubApp `MyLib::sys::Softkey_Aux_IE` – SoftKey oder AUX
- **METHOD_SERVER**: `iec61499::net::SERVER_0` (`QI=TRUE`) – lokale Methode
- **MERGE**: `iec61499::events::E_MERGE_2` – führt die beiden Auslöser zusammen

## Programmablauf und Verbindungen

- `SoftkeyAux.IND` → `MERGE.EI1`
- `METHOD_SERVER.IND` → `MERGE.EI2`
- `METHOD_SERVER.IND` → `METHOD_SERVER.RSP` (Rückverdrahtung)
- `MERGE.EO` → `IND`
- `u16ObjId`, `u16ObjIdA` → `SoftkeyAux`; `ID_METHOD` → `METHOD_SERVER.ID`

## Technische Besonderheiten

- **`RSP` muss auf `IND` zurückverdrahtet werden.** Der Trigger ist argumentlos und braucht keine Verarbeitung; ohne diese Verbindung blockiert FORTEs `onServerMethodCall` den OPC-UA-Server-Thread bis zu 4 s und hält dabei den globalen `serviceMutex` von open62541, den jeder `AX_PUBLISH_1` auf einem anderen FORTE-Thread ebenfalls braucht (siehe `Ventilsteuerung/OPC_UA_VERNETZUNG.md`, Punkt 12).

## Anwendungsszenarien

- Taste, Joystick-Zuweisung und Web-/Dashboard-Aufruf, die dieselbe Aktion auslösen (z. B. „Alles Aus“)

## Fazit

`Softkey_Aux_Method_IE` vereint SoftKey, AUX und einen OPC-UA-Methodenaufruf zu einem gemeinsamen `IND`-Ereignis.

# Softkey_Aux_Auto_IXA_TO_Remote_WRITE_BG_OPC

![Softkey_Aux_Auto_IXA_TO_Remote_WRITE_BG_OPC_network](./Softkey_Aux_Auto_IXA_TO_Remote_WRITE_BG_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`Softkey_Aux_Auto_IXA_TO_Remote_WRITE_BG_OPC` ist ein dünnwandiger Wrapper (ein Kanal), der zwei SubApps nebeneinanderstellt:

- **Command** (`Softkey_Aux_Auto_IXA_TO_Remote_WRITE`): VT-SoftKey lesen ODER AUX-Zuweisung (Joystick) lesen ODER lokaler Web-Override ODER Automatik-Kommando → Remote-OPC-UA-Write an das Zielmodul.
- **Status** (`AX_SUBSCRIBE_BG3_WEB_OPC`): VT-Hintergrundfarbe (SoftKey UND AUX) aus einem Remote-Subscribe sowie lokaler Web-Republish (unverändert).

Der Baustein ist eine Kopie von `Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC`, die als Kommandohälfte `Softkey_Aux_Auto_IXA_TO_Remote_WRITE` statt `Softkey_Aux_IXA_TO_Remote_WRITE` verwendet. Er ist nur für Kanäle mit Automatik-Bedarf gedacht (aktuell: Achslenkung links/rechts, Hangausgleich links/rechts); für alle anderen bleibt `Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC` zuständig.

![Softkey_Aux_Auto_IXA_TO_Remote_WRITE_BG_OPC](Softkey_Aux_Auto_IXA_TO_Remote_WRITE_BG_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung

Die Nutzereignisse kommen weiterhin über den Adapter-Socket.

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung (von `Status.INITO`)

### **Daten-Eingänge**

- **QI** (`BOOL`, Vorgabe `TRUE`): Quality Input – TRUE gibt den Dienst frei
- **u16ObjId** (`UINT`): Object-ID SoftKey/Hintergrund (VT)
- **u16ObjIdA** (`UINT`): Object-ID AuxFunction2/Hintergrund (VT)
- **ID_SUBSCRIBE** (`WSTRING`): Remote-Subscribe-Adresse (Status/Farbe, `ACTION=SUBSCRIBE`)
- **ID_WRITE_REMOTE** (`WSTRING`): Remote-Write-Adresse zum Zielmodul (Befehl, `ACTION=WRITE`, CLIENT)
- **ID_WEB_READ** (`WSTRING`): Lokale Subscribe-Adresse für einen Web-Client (z. B. `vt-ui-mirror`) auf demselben Modul, ODER-verknüpft mit SoftKey und AUX
- **ID_STATUS_WEB** (`WSTRING`): Lokale Publish-Adresse (auf diesem Modul) für den vom Zielmodul zurückgemeldeten Zustand, damit ein Web-Client dieselbe Hintergrundfarbe wie das echte VT zeigen kann

### **Daten-Ausgänge**

- **QO** (`BOOL`): Quality Output – UND aus `Command.QO` und `Status.QO`

### **Adapter**

- **AUTO_IN** (Socket, `adapter::types::unidirectional::AX`): Autonomes Kommando (z. B. Achslenkung-Mittelpunkt-Automatik), ODER-verknüpft mit SoftKey/Aux/Web
- **WEB_STATE** (Plug, `adapter::types::unidirectional::AX`): `Command.WEB_STATE` durchgereicht, damit andere Composite-Instanzen denselben Web-Zustand ohne eigenen lokalen Self-Subscribe nutzen können

## Verwendete Funktionsbausteine (FBs)

- **Command**: SubApp `MyLib::sys::Softkey_Aux_Auto_IXA_TO_Remote_WRITE`
- **Status**: SubApp `MyLib::sys::AX_SUBSCRIBE_BG3_WEB_OPC`
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_2`): verknüpft `Command.QO` und `Status.QO` zum `QO`

## Programmablauf und Verbindungen

- `u16ObjId`, `u16ObjIdA` → `Command` und `Status`
- `ID_WRITE_REMOTE`, `ID_WEB_READ` → `Command`
- `ID_SUBSCRIBE`, `ID_STATUS_WEB` → `Status`
- `AUTO_IN` → `Command.AUTO_IN`
- `Command.WEB_STATE` → `WEB_STATE`
- `INIT` → `Command.INIT`; `Command.INITO` → `Status.INIT`; `Status.INITO` → `INITO` (und → `AND_QO.REQ`)
- `QI` → `Command.QI` und `Status.QI`
- `Command.QO` → `AND_QO.IN1`; `Status.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`

`Command` und `Status` sind nicht direkt miteinander verbunden; die Rückmeldung läuft über den Remote-Subscribe.

## Technische Besonderheiten

- `WEB_STATE` (Version 1.1 von `Softkey_Aux_Auto_IXA_TO_Remote_WRITE`) ist ein durchgereichter Fan-out-Plug.
- Nur für Kanäle mit Automatik-Bedarf verwenden.

## Anwendungsszenarien

- VT-Taste/AUX/Web/Automatik, die einen Befehl per OPC-UA an ein Zielmodul schreibt, mit Hintergrundfarbe aus dem zurückgemeldeten Zustand (z. B. Achslenkung, Hangausgleich)

## Fazit

`Softkey_Aux_Auto_IXA_TO_Remote_WRITE_BG_OPC` kombiniert Kommando (inkl. Automatik) und Statusrückmeldung eines Kanals in einer SubApp.

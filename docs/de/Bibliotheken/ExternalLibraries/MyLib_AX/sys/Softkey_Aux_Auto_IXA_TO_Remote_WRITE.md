# Softkey_Aux_Auto_IXA_TO_Remote_WRITE

![Softkey_Aux_Auto_IXA_TO_Remote_WRITE_network](./Softkey_Aux_Auto_IXA_TO_Remote_WRITE_network.svg)

* * * * * * * * * *

## Einleitung

`Softkey_Aux_Auto_IXA_TO_Remote_WRITE` liest einen **VT-SoftKey**, eine **AUX-Zuweisung (Joystick)**, einen **lokalen Web-Override** oder ein **Automatik-Kommando** und schreibt das Ergebnis per Remote-OPC-UA-Write an ein Zielmodul (generisch, ein Kanal, ohne Status-Feedback). Der Baustein ist eine Kopie von [`Softkey_Aux_IXA_TO_Remote_WRITE`](./Softkey_Aux_IXA_TO_Remote_WRITE.md) mit einem zusätzlichen Socket `AUTO_IN` für Funktionen mit autonomer Ansteuerung (z. B. Achslenkung-Mittelpunkt-Automatik).

Es gibt weiterhin genau **einen** Publisher pro Kanal auf `ID_WRITE_REMOTE`, auch wenn vier statt drei Quellen zusammengeführt werden – kein Self-Latch-Risiko. Für Status-Feedback/Hintergrundfarbe siehe die vorverdrahtete Kombination [`Softkey_Aux_Auto_IXA_TO_Remote_WRITE_BG_OPC`](./Softkey_Aux_Auto_IXA_TO_Remote_WRITE_BG_OPC.md).

![Softkey_Aux_Auto_IXA_TO_Remote_WRITE](Softkey_Aux_Auto_IXA_TO_Remote_WRITE.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: Softkey_Aux_Auto_IXA_TO_Remote_WRITE

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **Softkey_IXA**: `isobus::UT::io::Softkey::Softkey_IXA` (`QI=TRUE`) – SoftKey-Pegel
    - **Aux_IXA**: `isobus::UT::io::Auxiliary::IN::Aux_IXA` (`QI=TRUE`) – AUX-Pegel
    - **SUBSCRIBE_WEB**: `adapter::net::AX_SUBSCRIBE_1` (`QI=TRUE`) – lokaler Subscribe für einen Web-Client
    - **SPLIT_WEB**: `adapter::events::unidirectional::AX_SPLIT_2` – verteilt den Web-Zustand auf `OR_MERGE` und `WEB_STATE`
    - **OR_MERGE**: `adapter::booleanOperators::AX_OR_4` – ODER-Verknüpfung der vier Quellen
    - **AX_CLIENT_1_0**: `adapter::net::AX_CLIENT_1_0` (`QI=TRUE`) – Remote-Write an das Zielmodul
- **Import**: `ID_NULL`

## Schnittstellenstruktur

### **Ereignis-Eingänge / Ereignis-Ausgänge**

Keine – Ereignisse laufen über die Adapter.

### **Daten-Eingänge**

- **u16ObjId** (`UINT`, Vorgabe `ID_NULL`): Object-ID SoftKey (VT)
- **u16ObjIdA** (`UINT`, Vorgabe `ID_NULL`): Object-ID `AuxFunction2` (VT)
- **ID_WRITE_REMOTE** (`WSTRING`): Remote-Write-Adresse zum Zielmodul (Befehl, `ACTION=WRITE`, `CLIENT`)
- **ID_WEB_READ** (`WSTRING`): Lokale Subscribe-Adresse für einen Web-Client (z. B. `vt-ui-mirror`) auf demselben Modul, ODER-verknüpft mit SoftKey und AUX

### **Daten-Ausgänge**

Keine.

### **Adapter**

- **AUTO_IN** (Socket, `adapter::types::unidirectional::AX`): Autonomes Kommando (z. B. Achslenkung-Mittelpunkt-Automatik), ODER-verknüpft mit SoftKey/Aux/Web
- **WEB_STATE** (Plug, `adapter::types::unidirectional::AX`): Web-Zustand (= `SUBSCRIBE_WEB.OUT`) nach außen durchgereicht, damit andere Composite-Instanzen denselben Wert per Datenverbindung nutzen können, statt selbst nochmals lokal zu abonnieren (nur ein lokaler OPC-UA-Subscriber pro Knoten zulässig)

## Programmablauf und Verbindungen

- `u16ObjId` → `Softkey_IXA.u16ObjId`; `Softkey_IXA.IN` → `OR_MERGE.IN1`
- `u16ObjIdA` → `Aux_IXA.u16ObjId`; `Aux_IXA.IN` → `OR_MERGE.IN2`
- `ID_WEB_READ` → `SUBSCRIBE_WEB.ID`; `SUBSCRIBE_WEB.OUT` → `SPLIT_WEB.IN`
- `SPLIT_WEB.OUT1` → `OR_MERGE.IN3`; `SPLIT_WEB.OUT2` → `WEB_STATE`
- `AUTO_IN` → `OR_MERGE.IN4`
- `OR_MERGE.OUT` → `AX_CLIENT_1_0.IN`; `ID_WRITE_REMOTE` → `AX_CLIENT_1_0.ID`

## Technische Besonderheiten

- Version 1.2 (2026-09-16): `SPLIT_WEB` (`AX_SPLIT_2`) zwischengesetzt, da Adapter-Plugs strikt 1:1 sind (kein Fan-out wie bei Daten-/Ereignisverbindungen); zwei Ziele am selben Plug führten zu einem FORTE-Bootfehler (`INVALID_STATE` beim zweiten `CREATE Connection`)
- Version 1.1 (2026-09-16): Plug `WEB_STATE` ergänzt; behebt den Doppel-Self-Subscribe auf `ID_WEB_READ` (FORTE: „Cannot add another one")
- Version 1.0 (2026-09-11): Kopie von `Softkey_Aux_IXA_TO_Remote_WRITE` mit `AX_OR_4` statt `AX_OR_3`; nur für Kanäle mit Automatik-Bedarf verwenden, alle anderen Verwender bleiben auf dem Originaltyp
- Kein Status-Feedback und keine Hintergrundfarbe in diesem Baustein

## Anwendungsszenarien

- Remote-Kommando, das gleichzeitig per SoftKey, Joystick, Web-Client und autonomer Automatik ausgelöst werden kann (z. B. Achslenkung-Mittelpunkt-Automatik)
- Quelle des `WEB_STATE` für [`Softkey_Aux_Web_IE`](./Softkey_Aux_Web_IE.md) (Override-Erkennung)

## Zusammenfassung

`Softkey_Aux_Auto_IXA_TO_Remote_WRITE` verknüpft SoftKey, AUX, Web-Override und Automatik per ODER und schreibt das Ergebnis über genau einen Publisher an das Zielmodul.

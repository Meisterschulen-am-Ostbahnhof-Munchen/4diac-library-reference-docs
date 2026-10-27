# SRT_RPC_FROM_Remote_QXA_OPC_LOC

![SRT_RPC_FROM_Remote_QXA_OPC_LOC_network](./SRT_RPC_FROM_Remote_QXA_OPC_LOC_network.svg)

* * * * * * * * * *

## Einleitung

`SRT_RPC_FROM_Remote_QXA_OPC_LOC` ist die lokale Variante von [`SRT_RPC_FROM_Remote_QXA_OPC`](./SRT_RPC_FROM_Remote_QXA_OPC.md): Statt den Zustand per Remote-Write über einen `AX_CLIENT_1_0` zu schreiben, wird er lokal per `AX_PUBLISH_1` geschrieben (ein Gerät, Methoden per `localhost`, kein Endpunkt). Die SubApp empfängt über drei separate `SERVER_0`-Methodenaufrufe (Set, Reset, Toggle) Trigger, steuert damit ein Set/Reset/Toggle-Flipflop (`AX_T_FF_SR`), schaltet den Digitalausgang `DigitalOutput_Q1` und veröffentlicht den neuen Zustand lokal unter `ID_STATE_WRITE`.

![SRT_RPC_FROM_Remote_QXA_OPC_LOC](SRT_RPC_FROM_Remote_QXA_OPC_LOC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `INIT` (`EInit`): Initialization Request

### **Ereignis-Ausgänge**

- `INITO` (`EInit`): Initialization Confirm

### **Daten-Eingänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `QI` | `BOOL` | Quality Input - TRUE, um den Dienst freizugeben. Initialwert `TRUE`. |
| `Output` | `logiBUS::io::DQ::logiBUS_DO_S` | Identifiziert den Ausgang `Output_Q1..Q8`. Initialwert `logiBUS_DO::Invalid`. |
| `ID_SET_METHOD` | `WSTRING` | Lokale Methodenadresse (`ACTION=CREATE_METHOD`) für den Set-Methodenaufruf, wird von Gerät A per `CALL_METHOD` aufgerufen. |
| `ID_RESET_METHOD` | `WSTRING` | Lokale Methodenadresse (`ACTION=CREATE_METHOD`) für den Reset-Methodenaufruf, wird von Gerät A per `CALL_METHOD` aufgerufen. |
| `ID_TOGGLE_METHOD` | `WSTRING` | Lokale Methodenadresse (`ACTION=CREATE_METHOD`) für den Toggle-Methodenaufruf, wird von Gerät A per `CALL_METHOD` aufgerufen. |
| `ID_STATE_WRITE` | `WSTRING` | Lokale Knotenadresse (`BOOL`, `ACTION=WRITE`) für den Zustand. |

### **Daten-Ausgänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `QO` | `BOOL` | Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde. |

### **Adapter**

Keine vorhanden.

## Verwendete Funktionsbausteine (FBs)

- **TRIGGER_SET_SERVER / TRIGGER_RESET_SERVER / TRIGGER_TOGGLE_SERVER** (`iec61499::net::SERVER_0`): empfangen je einen eigenen Methodenaufruf (`ID_SET_METHOD` / `ID_RESET_METHOD` / `ID_TOGGLE_METHOD`).
- **AX_T_FF_SR** (`adapter::events::unidirectional::AX_T_FF_SR`): Flipflop mit den Eingängen `S`, `R` und `CLK` (Toggle).
- **AX_SPLIT_2** (`adapter::events::unidirectional::AX_SPLIT_2`): verzweigt den Zustand `Q` auf physischen Ausgang und lokales Publish.
- **DigitalOutput_Q1** (`logiBUS::io::DQ::logiBUS_QXA`): physischer Ausgang.
- **STATE_PUBLISH** (`adapter::net::AX_PUBLISH_1`): schreibt den Zustand lokal unter `ID_STATE_WRITE`.
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_5`): verknüpft die `QO` der drei Server, von `DigitalOutput_Q1` und von `STATE_PUBLISH`.

## Programmablauf und Verbindungen

1. Flipflop-Ansteuerung: `TRIGGER_SET_SERVER.IND` → `AX_T_FF_SR.S`; `TRIGGER_RESET_SERVER.IND` → `AX_T_FF_SR.R`; `TRIGGER_TOGGLE_SERVER.IND` → `AX_T_FF_SR.CLK`.
2. Antwort: `TRIGGER_SET_SERVER.IND` → `TRIGGER_SET_SERVER.RSP`; `TRIGGER_RESET_SERVER.IND` → `TRIGGER_RESET_SERVER.RSP`; `TRIGGER_TOGGLE_SERVER.IND` → `TRIGGER_TOGGLE_SERVER.RSP`.
3. Initialisierungskette: `INIT` → `TRIGGER_SET_SERVER.INIT`; `TRIGGER_SET_SERVER.INITO` → `TRIGGER_RESET_SERVER.INIT`; `TRIGGER_RESET_SERVER.INITO` → `TRIGGER_TOGGLE_SERVER.INIT`; `TRIGGER_TOGGLE_SERVER.INITO` → `DigitalOutput_Q1.INIT`; `DigitalOutput_Q1.INITO` → `STATE_PUBLISH.INIT`; `STATE_PUBLISH.INITO` → `INITO` und → `AND_QO.REQ`.
4. Zustand: `AX_T_FF_SR.Q` → `AX_SPLIT_2.IN`; `AX_SPLIT_2.OUT1` → `DigitalOutput_Q1.OUT`; `AX_SPLIT_2.OUT2` → `STATE_PUBLISH.IN`.
5. Daten: `Output` → `DigitalOutput_Q1.Output`; `ID_SET_METHOD` → `TRIGGER_SET_SERVER.ID`; `ID_RESET_METHOD` → `TRIGGER_RESET_SERVER.ID`; `ID_TOGGLE_METHOD` → `TRIGGER_TOGGLE_SERVER.ID`; `ID_STATE_WRITE` → `STATE_PUBLISH.ID`.
6. Qualifier: `QI` → `TRIGGER_SET_SERVER.QI`, `TRIGGER_RESET_SERVER.QI`, `TRIGGER_TOGGLE_SERVER.QI`, `DigitalOutput_Q1.QI` und `STATE_PUBLISH.QI`.
7. Qualitätsauswertung: `TRIGGER_SET_SERVER.QO` → `AND_QO.IN1`; `TRIGGER_RESET_SERVER.QO` → `AND_QO.IN2`; `TRIGGER_TOGGLE_SERVER.QO` → `AND_QO.IN3`; `DigitalOutput_Q1.QO` → `AND_QO.IN4`; `STATE_PUBLISH.QO` → `AND_QO.IN5`; `AND_QO.OUT` → `QO`.

## Technische Besonderheiten

- Gegenüber der Remote-Variante ersetzt `STATE_PUBLISH` (`AX_PUBLISH_1`) den `STATE_CLIENT` (`AX_CLIENT_1_0`); die Initialisierungskette, die Methoden-Server und die Flipflop-Logik sind unverändert.
- Die `RSP`-Ereignisse der drei Server sind direkt mit den jeweiligen `IND`-Ereignissen verbunden.

## Anwendungsszenarien

- Lokale Übung auf einem Gerät: Set, Reset und Toggle werden per OPC-UA-Methodenaufruf (`localhost`) ausgelöst, der Zustand wird lokal veröffentlicht.
- Testaufbau der Methodenkette ohne zweites Gerät als Gegenstück zu [`SRT_RPC_FROM_Remote_QXA_OPC`](./SRT_RPC_FROM_Remote_QXA_OPC.md).

## Zusammenfassung

`SRT_RPC_FROM_Remote_QXA_OPC_LOC` steuert ein Set/Reset/Toggle-Flipflop per drei Methodenaufrufen, schaltet `DigitalOutput_Q1` und veröffentlicht den Zustand lokal per `AX_PUBLISH_1`.

## 🛠️ Zugehörige Übungen

- [Uebung_OPC_L04_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Meins/OPC_UA_Lokal/Uebung_OPC_L04_AX/)

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

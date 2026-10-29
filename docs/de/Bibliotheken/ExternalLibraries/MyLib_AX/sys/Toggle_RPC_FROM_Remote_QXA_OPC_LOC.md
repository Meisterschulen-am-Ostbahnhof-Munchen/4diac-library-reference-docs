# Toggle_RPC_FROM_Remote_QXA_OPC_LOC

![Toggle_RPC_FROM_Remote_QXA_OPC_LOC_network](./Toggle_RPC_FROM_Remote_QXA_OPC_LOC_network.svg)

* * * * * * * * * *

## Einleitung

`Toggle_RPC_FROM_Remote_QXA_OPC_LOC` ist ein Toggle-Flipflop per `SERVER_0`-Trigger auf `Output_Q1`, dessen Zustand lokal per `AX_PUBLISH_1` veröffentlicht wird. Es ist die lokale Variante von [`Toggle_RPC_FROM_Remote_QXA_OPC`](./Toggle_RPC_FROM_Remote_QXA_OPC.md): Statt den Zustand per `AX_CLIENT_1_0` remote zurückzuschreiben, wird er lokal per `PUBLISH` geschrieben (kein Endpunkt). Der Trigger ist ein argumentloser Methodenaufruf auf `ID_TRIGGER_METHOD`; jeder Aufruf taktet das Toggle-Flipflop `AX_T_FF`, schaltet `DigitalOutput_Q1` und veröffentlicht den Zustand unter `ID_STATE_WRITE`.

![Toggle_RPC_FROM_Remote_QXA_OPC_LOC](Toggle_RPC_FROM_Remote_QXA_OPC_LOC.svg)

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
| `ID_TRIGGER_METHOD` | `WSTRING` | Lokale Methodenadresse (`ACTION=CREATE_METHOD`) für den argumentlosen Trigger-Methodenaufruf, wird von Gerät A per `CALL_METHOD` aufgerufen. |
| `ID_STATE_WRITE` | `WSTRING` | Lokale Knotenadresse (`BOOL`, `ACTION=WRITE`) für den Flipflop-Zustand. |

### **Daten-Ausgänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `QO` | `BOOL` | Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde. |

### **Adapter**

Keine vorhanden.

## Verwendete Funktionsbausteine (FBs)

- **TRIGGER_SERVER** (`iec61499::net::SERVER_0`): empfängt den Trigger-Methodenaufruf (`ID_TRIGGER_METHOD`).
- **AX_T_FF** (`adapter::events::unidirectional::AX_T_FF`): Toggle-Flipflop, getaktet über `CLK`.
- **AX_SPLIT_2** (`adapter::events::unidirectional::AX_SPLIT_2`): verzweigt den Zustand auf physischen Ausgang und lokales Publish.
- **DigitalOutput_Q1** (`logiBUS::io::DQ::logiBUS_QXA`): physischer Ausgang.
- **STATE_PUBLISH** (`adapter::net::AX_PUBLISH_1`): schreibt den Zustand lokal unter `ID_STATE_WRITE`.
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_3`): verknüpft die `QO` von `TRIGGER_SERVER`, `DigitalOutput_Q1` und `STATE_PUBLISH`.

## Programmablauf und Verbindungen

1. Trigger: `TRIGGER_SERVER.IND` → `AX_T_FF.CLK`; `TRIGGER_SERVER.IND` → `TRIGGER_SERVER.RSP`.
2. Initialisierungskette: `INIT` → `TRIGGER_SERVER.INIT`; `TRIGGER_SERVER.INITO` → `DigitalOutput_Q1.INIT`; `DigitalOutput_Q1.INITO` → `STATE_PUBLISH.INIT`; `STATE_PUBLISH.INITO` → `INITO` und → `AND_QO.REQ`.
3. Zustand: `AX_T_FF.Q` → `AX_SPLIT_2.IN`; `AX_SPLIT_2.OUT1` → `DigitalOutput_Q1.OUT`; `AX_SPLIT_2.OUT2` → `STATE_PUBLISH.IN`.
4. Daten: `Output` → `DigitalOutput_Q1.Output`; `ID_TRIGGER_METHOD` → `TRIGGER_SERVER.ID`; `ID_STATE_WRITE` → `STATE_PUBLISH.ID`.
5. Qualifier: `QI` → `TRIGGER_SERVER.QI`, `DigitalOutput_Q1.QI` und `STATE_PUBLISH.QI`.
6. Qualitätsauswertung: `TRIGGER_SERVER.QO` → `AND_QO.IN1`; `DigitalOutput_Q1.QO` → `AND_QO.IN2`; `STATE_PUBLISH.QO` → `AND_QO.IN3`; `AND_QO.OUT` → `QO`.

## Technische Besonderheiten

- Gegenüber der Remote-Variante ersetzt `STATE_PUBLISH` (`AX_PUBLISH_1`) den `STATE_CLIENT` (`AX_CLIENT_1_0`).
- `TRIGGER_SERVER.IND` ist direkt mit `TRIGGER_SERVER.RSP` verbunden.

## Anwendungsszenarien

- Lokale Übung auf einem Gerät: Ein Methodenaufruf schaltet einen Ausgang um, der Zustand wird lokal veröffentlicht.
- Gegenstück zu [`Input_Toggle_RPC_TO_Remote_QXA_OPC`](./Input_Toggle_RPC_TO_Remote_QXA_OPC.md), das die Toggle-Methode per logiBUS-Taster aufruft.

## Zusammenfassung

`Toggle_RPC_FROM_Remote_QXA_OPC_LOC` schaltet `DigitalOutput_Q1` bei jedem Trigger-Methodenaufruf um und veröffentlicht den Zustand lokal per `AX_PUBLISH_1`.

## 🛠️ Zugehörige Übungen

- [Uebung_OPC_L03_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Meins/OPC_UA_Lokal/Uebung_OPC_L03_AX/)
- [Uebung_OPC_L08_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Meins/OPC_UA_Lokal/Uebung_OPC_L08_AX/)

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

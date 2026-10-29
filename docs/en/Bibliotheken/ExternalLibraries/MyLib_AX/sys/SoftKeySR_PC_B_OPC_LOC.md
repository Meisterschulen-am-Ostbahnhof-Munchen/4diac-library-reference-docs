# SoftKeySR_PC_B_OPC_LOC

![SoftKeySR_PC_B_OPC_LOC_network](./SoftKeySR_PC_B_OPC_LOC_network.svg)

* * * * * * * * * *

## Introduction

`SoftKeySR_PC_B_OPC_LOC` is the local variant of [`SoftKeySR_PC_B_OPC`](./SoftKeySR_PC_B_OPC.md): instead of writing the state remotely via an `AX_CLIENT_1_0`, it is written locally via `AX_PUBLISH_1` (single device, methods via `localhost`, no endpoint). The SubApp receives set and reset each via its own `SERVER_0` method call, clocks the pure SR flip-flop logic (`AX_SR`, no toggle), switches `DigitalOutput_Q1` and publishes the new state locally at `ID_STATE_WRITE`.

![SoftKeySR_PC_B_OPC_LOC](SoftKeySR_PC_B_OPC_LOC.svg)

## Interface Structure

### **Event Inputs**

- `INIT` (`EInit`): Initialization Request

### **Event Outputs**

- `INITO` (`EInit`): Initialization Confirm

### **Data Inputs**

| Name | Type | Description |
|------|-----|--------------|
| `QI` | `BOOL` | Quality Input - TRUE to enable the service. Initial value `TRUE`. |
| `Output` | `logiBUS::io::DQ::logiBUS_DO_S` | Identifies the output `Output_Q1..Q8`. Initial value `logiBUS_DO::Invalid`. |
| `ID_SET_METHOD` | `WSTRING` | Local method address (`ACTION=CREATE_METHOD`) for the Set method call, called by device A via `CALL_METHOD`. |
| `ID_RESET_METHOD` | `WSTRING` | Local method address (`ACTION=CREATE_METHOD`) for the Reset method call, called by device A via `CALL_METHOD`. |
| `ID_STATE_WRITE` | `WSTRING` | Local node address (`BOOL`, `ACTION=WRITE`) for the state. |

### **Data Outputs**

| Name | Type | Description |
|------|-----|--------------|
| `QO` | `BOOL` | Quality Output - TRUE if the service was executed successfully. |

### **Adapters**

None.

## Function Blocks (FBs) Used

- **TRIGGER_SET_SERVER / TRIGGER_RESET_SERVER** (`iec61499::net::SERVER_0`): each receives its own method call (`ID_SET_METHOD` / `ID_RESET_METHOD`).
- **AX_SR** (`adapter::events::unidirectional::AX_SR`): pure set/reset flip-flop logic, no toggle.
- **AX_SPLIT_2** (`adapter::events::unidirectional::AX_SPLIT_2`): branches the state to the physical output and the local publish.
- **DigitalOutput_Q1** (`logiBUS::io::DQ::logiBUS_QXA`): physical output.
- **STATE_PUBLISH** (`adapter::net::AX_PUBLISH_1`): writes the state locally at `ID_STATE_WRITE`.
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_4`): combines the `QO` of the two servers, of `DigitalOutput_Q1` and of `STATE_PUBLISH`.

## Program Flow and Connections

1. Flip-flop control: `TRIGGER_SET_SERVER.IND` → `AX_SR.S`; `TRIGGER_RESET_SERVER.IND` → `AX_SR.R`.
2. Response: `TRIGGER_SET_SERVER.IND` → `TRIGGER_SET_SERVER.RSP`; `TRIGGER_RESET_SERVER.IND` → `TRIGGER_RESET_SERVER.RSP`.
3. Initialization chain: `INIT` → `TRIGGER_SET_SERVER.INIT`; `TRIGGER_SET_SERVER.INITO` → `TRIGGER_RESET_SERVER.INIT`; `TRIGGER_RESET_SERVER.INITO` → `DigitalOutput_Q1.INIT`; `DigitalOutput_Q1.INITO` → `STATE_PUBLISH.INIT`; `STATE_PUBLISH.INITO` → `INITO` and → `AND_QO.REQ`.
4. State: `AX_SR.Q` → `AX_SPLIT_2.IN`; `AX_SPLIT_2.OUT1` → `DigitalOutput_Q1.OUT`; `AX_SPLIT_2.OUT2` → `STATE_PUBLISH.IN`.
5. Data: `Output` → `DigitalOutput_Q1.Output`; `ID_SET_METHOD` → `TRIGGER_SET_SERVER.ID`; `ID_RESET_METHOD` → `TRIGGER_RESET_SERVER.ID`; `ID_STATE_WRITE` → `STATE_PUBLISH.ID`.
6. Qualifier: `QI` → `TRIGGER_SET_SERVER.QI`, `TRIGGER_RESET_SERVER.QI`, `DigitalOutput_Q1.QI` and `STATE_PUBLISH.QI`.
7. Quality evaluation: `TRIGGER_SET_SERVER.QO` → `AND_QO.IN1`; `TRIGGER_RESET_SERVER.QO` → `AND_QO.IN2`; `DigitalOutput_Q1.QO` → `AND_QO.IN3`; `STATE_PUBLISH.QO` → `AND_QO.IN4`; `AND_QO.OUT` → `QO`.

## Technical Features

- Compared with the remote variant, `STATE_PUBLISH` (`AX_PUBLISH_1`) replaces `STATE_CLIENT` (`AX_CLIENT_1_0`).
- The `RSP` events of the two servers are connected directly to the respective `IND` events.

## Application Scenarios

- Local exercise on a single device: set and reset are triggered by OPC UA method calls (`localhost`), the state is published locally.
- Test setup without a second device, as counterpart to [`SoftKeySR_PC_B_OPC`](./SoftKeySR_PC_B_OPC.md).

## Summary

`SoftKeySR_PC_B_OPC_LOC` controls a pure SR flip-flop via two method calls, switches `DigitalOutput_Q1` and publishes the state locally via `AX_PUBLISH_1`.

## 🛠️ Related Exercises

- [Uebung_OPC_L05_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Meins/OPC_UA_Lokal/Uebung_OPC_L05_AX/)

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

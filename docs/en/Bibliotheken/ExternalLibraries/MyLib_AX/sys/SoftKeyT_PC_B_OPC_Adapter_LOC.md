# SoftKeyT_PC_B_OPC_Adapter_LOC

![SoftKeyT_PC_B_OPC_Adapter_LOC_network](./SoftKeyT_PC_B_OPC_Adapter_LOC_network.svg)

* * * * * * * * * *

## Introduction

`SoftKeyT_PC_B_OPC_Adapter_LOC` is the local variant of [`SoftKeyT_PC_B_OPC_Adapter`](./SoftKeyT_PC_B_OPC_Adapter.md): instead of writing the state remotely via an `AX_CLIENT_1_0`, it is written locally via `AX_PUBLISH_1` (single device, methods via `localhost`, no endpoint). Server and publish are bundled in the SubApp [`MyLib::net::AE_AX_SERVER_0_PUBLISH_1`](../net/AE_AX_SERVER_0_PUBLISH_1.md) behind a single bidirectional adapter connection (`TRIGGER`); `AE_AX_AX_SPLIT` feeds both `DigitalOutput_Q1` and the toggle flip-flop logic (`AE_AX_T_FF`).

![SoftKeyT_PC_B_OPC_Adapter_LOC](SoftKeyT_PC_B_OPC_Adapter_LOC.svg)

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
| `ID_TRIGGER_METHOD` | `WSTRING` | Local method address (`ACTION=CREATE_METHOD`) for the argument-less trigger method call, called by device A via `CALL_METHOD`. |
| `ID_STATE_WRITE` | `WSTRING` | Local node address (`BOOL`, `ACTION=WRITE`) for the state. |

### **Data Outputs**

| Name | Type | Description |
|------|-----|--------------|
| `QO` | `BOOL` | Quality Output - TRUE if the service was executed successfully. |

### **Adapters**

None.

## Function Blocks (FBs) Used

- **TRIGGER** (SubApp `MyLib::net::AE_AX_SERVER_0_PUBLISH_1`): bundles the method server (`ID`) and the local publish of the state (`ID_STATE`); plug `TRIGGER` (`adapter::types::bidirectional::AE_AX`).
- **SPLIT** (`adapter::events::bidirectional::AE_AX_AX_SPLIT`): branches the trigger to the physical output and the flip-flop logic.
- **FLIPFLOP** (`adapter::events::bidirectional::AE_AX_T_FF`): toggle flip-flop logic as an adapter block (input `CLK`).
- **DigitalOutput_Q1** (`logiBUS::io::DQ::logiBUS_QXA`): physical output.
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_2`): combines `TRIGGER.QO` and `DigitalOutput_Q1.QO` into `QO`.

## Program Flow and Connections

1. Initialization: `INIT` → `TRIGGER.INIT`; `TRIGGER.INITO` → `DigitalOutput_Q1.INIT`; `DigitalOutput_Q1.INITO` → `INITO` and → `AND_QO.REQ`.
2. Adapter: `TRIGGER.TRIGGER` → `SPLIT.IN`; `SPLIT.OUT` → `FLIPFLOP.CLK`; `SPLIT.AX_OUT` → `DigitalOutput_Q1.OUT`.
3. Data: `Output` → `DigitalOutput_Q1.Output`; `ID_TRIGGER_METHOD` → `TRIGGER.ID`; `ID_STATE_WRITE` → `TRIGGER.ID_STATE`.
4. Qualifier: `QI` → `TRIGGER.QI` and `DigitalOutput_Q1.QI`.
5. Quality evaluation: `TRIGGER.QO` → `AND_QO.IN1`; `DigitalOutput_Q1.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`.

## Technical Features

- The local publish of the state is inside the SubApp `MyLib::net::AE_AX_SERVER_0_PUBLISH_1` (there `STATE_PUBLISH`, `AX_PUBLISH_1`, input `ID_STATE`), not in this SubApp.
- The contained SubApp [`MyLib::net::AE_AX_SERVER_0_PUBLISH_1`](../net/AE_AX_SERVER_0_PUBLISH_1.md) wires `IND` → `RSP` on every `SERVER_0`; rule see [SERVER_0](../../../StandardLibraries/net/SERVER_0.md).

## Application Scenarios

- Local exercise on a single device with adapter-bundled toggle logic and local state publish.

## Summary

Adapter-bundled local variant of [`SoftKeyT_PC_B_OPC_Adapter`](./SoftKeyT_PC_B_OPC_Adapter.md): method server, toggle flip-flop logic and local state publish behind the adapter `TRIGGER`.

## 🛠️ Related Exercises

- [Uebung_OPC_L09_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Meins/OPC_UA_Lokal/Uebung_OPC_L09_AX/)

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

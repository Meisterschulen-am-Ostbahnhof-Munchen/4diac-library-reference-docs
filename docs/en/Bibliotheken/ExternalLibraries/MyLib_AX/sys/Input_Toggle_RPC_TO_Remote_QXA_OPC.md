# Input_Toggle_RPC_TO_Remote_QXA_OPC

![Input_Toggle_RPC_TO_Remote_QXA_OPC_network](./Input_Toggle_RPC_TO_Remote_QXA_OPC_network.svg)

* * * * * * * * * *

## Introduction

`Input_Toggle_RPC_TO_Remote_QXA_OPC` corresponds to [`Softkey_Toggle_RPC_TO_Remote_BG_OPC`](./Softkey_Toggle_RPC_TO_Remote_BG_OPC.md), but without VT: pressing the button at `Input` calls the toggle method, the state is shown at `Output`. `logiBUS_IE` is used instead of `Softkey_IE`, and a `logiBUS_QXA` instead of `GreenWhiteBackground` - for tests without ISOBUS (Meins/OPC_UA_Lokal L03).

![Input_Toggle_RPC_TO_Remote_QXA_OPC](Input_Toggle_RPC_TO_Remote_QXA_OPC.svg)

## Interface Structure

### **Event Inputs**

- `INIT` (`EInit`): Initialization Request

### **Event Outputs**

- `INITO` (`EInit`): Initialization Confirm

### **Data Inputs**

| Name | Type | Description |
|------|-----|--------------|
| `QI` | `BOOL` | Quality Input - TRUE to enable the service. Initial value `TRUE`. |
| `Input` | `logiBUS::io::DI::logiBUS_DI_S` | Pushbutton `Input_I1..I8` (pressing the button calls the method). Initial value `logiBUS_DI::Invalid`. |
| `Output` | `logiBUS::io::DQ::logiBUS_DO_S` | Display of the flip-flop state `Output_Q1..Q8`. Initial value `logiBUS_DO::Invalid`. |
| `ID_TRIGGER_CALL` | `WSTRING` | Method address (`ACTION=CALL_METHOD`, with endpoint). |
| `ID_STATE_READ` | `WSTRING` | Local address (`BOOL`, `ACTION=READ`) for the state. |

### **Data Outputs**

| Name | Type | Description |
|------|-----|--------------|
| `QO` | `BOOL` | Quality Output - TRUE if the service was executed successfully. |

### **Adapters**

None.

## Function Blocks (FBs) Used

- **TRIGGER_INPUT** (`logiBUS::io::DI::logiBUS_IE`, parameter `InputEvent = BUTTON_PRESS_DOWN`): logiBUS pushbutton, event on pressing.
- **TRIGGER_CLIENT** (`iec61499::net::CLIENT_0`): calls the method `ID_TRIGGER_CALL`.
- **STATE_SUBSCRIBE** (`adapter::net::AX_SUBSCRIBE_1`): reads the state locally at `ID_STATE_READ`.
- **STATE_OUTPUT** (`logiBUS::io::DQ::logiBUS_QXA`): displays the state at the output `Output`.
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_4`): combines the `QO` of `TRIGGER_INPUT`, `TRIGGER_CLIENT`, `STATE_SUBSCRIBE` and `STATE_OUTPUT` into `QO`.

## Program Flow and Connections

1. Initialization chain: `INIT` → `TRIGGER_INPUT.INIT`; `TRIGGER_INPUT.INITO` → `TRIGGER_CLIENT.INIT`; `TRIGGER_CLIENT.INITO` → `STATE_SUBSCRIBE.INIT`; `STATE_SUBSCRIBE.INITO` → `STATE_OUTPUT.INIT`; `STATE_OUTPUT.INITO` → `INITO` and → `AND_QO.REQ`.
2. Method call: `TRIGGER_INPUT.IND` → `TRIGGER_CLIENT.REQ`.
3. Qualifier: `QI` → `TRIGGER_INPUT.QI`, `TRIGGER_CLIENT.QI`, `STATE_SUBSCRIBE.QI` and `STATE_OUTPUT.QI`.
4. Data: `Input` → `TRIGGER_INPUT.Input`; `Output` → `STATE_OUTPUT.Output`; `ID_TRIGGER_CALL` → `TRIGGER_CLIENT.ID`; `ID_STATE_READ` → `STATE_SUBSCRIBE.ID`.
5. State display: `STATE_SUBSCRIBE.OUT` → `STATE_OUTPUT.OUT`.
6. Quality evaluation: `TRIGGER_INPUT.QO` → `AND_QO.IN1`; `TRIGGER_CLIENT.QO` → `AND_QO.IN2`; `STATE_SUBSCRIBE.QO` → `AND_QO.IN3`; `STATE_OUTPUT.QO` → `AND_QO.IN4`; `AND_QO.OUT` → `QO`.

## Technical Features

- The state is not calculated in the block, but read locally from `ID_STATE_READ` and only displayed.
- Without `logiBUS_QXA`, the variant [`Input_Toggle_RPC_TO_Remote_AX_OPC`](./Input_Toggle_RPC_TO_Remote_AX_OPC.md) with AX plug `STATE` is available.

## Application Scenarios

- Testing the toggle RPC chain without an ISOBUS terminal: a logiBUS pushbutton calls the method, a logiBUS output shows the state.

## Summary

`Input_Toggle_RPC_TO_Remote_QXA_OPC` replaces the VT soft key and background colour of [`Softkey_Toggle_RPC_TO_Remote_BG_OPC`](./Softkey_Toggle_RPC_TO_Remote_BG_OPC.md) with a logiBUS pushbutton and a logiBUS output.

## 🛠️ Related Exercises

- [Uebung_OPC_L03_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Meins/OPC_UA_Lokal/Uebung_OPC_L03_AX/)

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

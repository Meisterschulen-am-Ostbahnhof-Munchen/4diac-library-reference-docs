# Input_Toggle_RPC_TO_Remote_AX_OPC

![Input_Toggle_RPC_TO_Remote_AX_OPC_network](./Input_Toggle_RPC_TO_Remote_AX_OPC_network.svg)

* * * * * * * * * *

## Introduction

`Input_Toggle_RPC_TO_Remote_AX_OPC` corresponds to [`Input_Toggle_RPC_TO_Remote_QXA_OPC`](./Input_Toggle_RPC_TO_Remote_QXA_OPC.md), but without `logiBUS_QXA`: the state is passed outwards as AX plug `STATE`, so display or further processing can be chosen freely. Pressing the button at the logiBUS input `Input` calls the toggle method `ID_TRIGGER_CALL` via `CLIENT_0`; the flip-flop state is read locally from `ID_STATE_READ` by `AX_SUBSCRIBE_1` and put directly on the plug `STATE`.

![Input_Toggle_RPC_TO_Remote_AX_OPC](Input_Toggle_RPC_TO_Remote_AX_OPC.svg)

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
| `ID_TRIGGER_CALL` | `WSTRING` | Method address (`ACTION=CALL_METHOD`, with endpoint). |
| `ID_STATE_READ` | `WSTRING` | Local address (`BOOL`, `ACTION=READ`) for the state. |

### **Data Outputs**

| Name | Type | Description |
|------|-----|--------------|
| `QO` | `BOOL` | Quality Output - TRUE if the service was executed successfully. |

### **Adapters**

| Adapter | Type | Direction | Description |
|---------|------|-----------|-------------|
| `STATE` | `adapter::types::unidirectional::AX` | Plug | Flip-flop state (read locally). |

## Function Blocks (FBs) Used

- **TRIGGER_INPUT** (`logiBUS::io::DI::logiBUS_IE`, parameter `InputEvent = BUTTON_PRESS_DOWN`): logiBUS pushbutton, event on pressing.
- **TRIGGER_CLIENT** (`iec61499::net::CLIENT_0`): calls the method `ID_TRIGGER_CALL`.
- **STATE_SUBSCRIBE** (`adapter::net::AX_SUBSCRIBE_1`): reads the state locally at `ID_STATE_READ`.
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_3`): combines `TRIGGER_INPUT.QO`, `TRIGGER_CLIENT.QO` and `STATE_SUBSCRIBE.QO` into `QO`.

## Program Flow and Connections

1. Initialization chain: `INIT` → `TRIGGER_INPUT.INIT`; `TRIGGER_INPUT.INITO` → `TRIGGER_CLIENT.INIT`; `TRIGGER_CLIENT.INITO` → `STATE_SUBSCRIBE.INIT`; `STATE_SUBSCRIBE.INITO` → `INITO` and → `AND_QO.REQ`.
2. Method call: `TRIGGER_INPUT.IND` → `TRIGGER_CLIENT.REQ`.
3. Qualifier: `QI` → `TRIGGER_INPUT.QI`, `TRIGGER_CLIENT.QI` and `STATE_SUBSCRIBE.QI`.
4. Data: `Input` → `TRIGGER_INPUT.Input`; `ID_TRIGGER_CALL` → `TRIGGER_CLIENT.ID`; `ID_STATE_READ` → `STATE_SUBSCRIBE.ID`.
5. State: `STATE_SUBSCRIBE.OUT` → plug `STATE`.
6. Quality evaluation: `TRIGGER_INPUT.QO` → `AND_QO.IN1`; `TRIGGER_CLIENT.QO` → `AND_QO.IN2`; `STATE_SUBSCRIBE.QO` → `AND_QO.IN3`; `AND_QO.OUT` → `QO`.

## Technical Features

- The block contains no physical output; the state is processed further via the plug `STATE`.
- The state is not calculated in the block, but read locally from `ID_STATE_READ`.

## Application Scenarios

- Pushbutton-controlled toggle on a target module without ISOBUS, where the state can be reused freely (e.g. for any display).
- Variant of [`Input_Toggle_RPC_TO_Remote_QXA_OPC`](./Input_Toggle_RPC_TO_Remote_QXA_OPC.md) when no `logiBUS_QXA` is needed as a display.

## Summary

`Input_Toggle_RPC_TO_Remote_AX_OPC` calls a toggle method via a logiBUS pushbutton and provides the locally read state as AX plug `STATE`.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

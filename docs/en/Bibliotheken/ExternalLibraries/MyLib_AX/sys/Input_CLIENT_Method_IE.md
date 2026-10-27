# Input_CLIENT_Method_IE

![Input_CLIENT_Method_IE_network](./Input_CLIENT_Method_IE_network.svg)

* * * * * * * * * *

## Introduction

`Input_CLIENT_Method_IE` is a logiBUS pushbutton (`logiBUS_IE`) that triggers an argument-less OPC UA method call (`CALL_METHOD`) via `CLIENT_0`. The block corresponds to `Button_CLIENT_Method_IE`, but uses `logiBUS_IE` instead of `Button_IE` (pushbutton instead of VT button). It was extracted from [`logiBUS_AI_Calibrated_Subscribe_VT_OPC`](./logiBUS_AI_Calibrated_Subscribe_VT_OPC.md), where the combination of `Button_IE` and `CLIENT_0` occurred twice in identical form. The triggering button event is the parameter `InputEvent` (not hard-coded to `BT_PRESSED_LATCHED`), so that other activation types are reusable as well.

![Input_CLIENT_Method_IE](Input_CLIENT_Method_IE.svg)

## Interface Structure

### **Event Inputs**

- `INIT` (`EInit`): Initialization Request

### **Event Outputs**

- `INITO` (`EInit`): Initialization Confirm

### **Data Inputs**

| Name | Type | Description |
|------|-----|--------------|
| `QI` | `BOOL` | Quality Input - TRUE to enable the service. Initial value `TRUE`. |
| `Input` | `logiBUS::io::DI::logiBUS_DI_S` | Identifies the input `Input_I1..I8`. Initial value `logiBUS_DI::Invalid`. |
| `InputEvent` | `logiBUS::io::DI::logiBUS_DI_Events_S` | Which button event triggers the call. Initial value `BUTTON_PRESS_DOWN`. |
| `ID_CALL` | `WSTRING` | Method address (`ACTION=CALL_METHOD`, with endpoint, also `localhost`). |

### **Data Outputs**

| Name | Type | Description |
|------|-----|--------------|
| `QO` | `BOOL` | Quality Output - TRUE if the service was executed successfully. |

### **Adapters**

None.

## Function Blocks (FBs) Used

- **Taster** (`logiBUS::io::DI::logiBUS_IE`): logiBUS pushbutton, delivers `IND` on the selected button event.
- **CLIENT** (`iec61499::net::CLIENT_0`): calls the method `ID_CALL` via `CALL_METHOD`.
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_2`): combines `Taster.QO` and `CLIENT.QO` into `QO`.

## Program Flow and Connections

1. Initialization: `INIT` → `Taster.INIT`; `Taster.INITO` → `CLIENT.INIT`; `CLIENT.INITO` → `INITO` and → `AND_QO.REQ`.
2. Method call: `Taster.IND` → `CLIENT.REQ`.
3. Qualifier: `QI` → `Taster.QI` and `CLIENT.QI`.
4. Data: `Input` → `Taster.Input`; `InputEvent` → `Taster.InputEvent`; `ID_CALL` → `CLIENT.ID`.
5. Quality evaluation: `Taster.QO` → `AND_QO.IN1`; `CLIENT.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`.

## Technical Features

- The button event is configurable via `InputEvent`, the default is `BUTTON_PRESS_DOWN`.
- The method address contains an endpoint; a call to `localhost` is also possible.

## Application Scenarios

- General pattern for calling an OPC UA method from a logiBUS pushbutton.
- Triggering a method on a button event other than pressing by adjusting `InputEvent`.

## Summary

`Input_CLIENT_Method_IE` calls an argument-less OPC UA method via `CLIENT_0` on the configured button event of a logiBUS pushbutton.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

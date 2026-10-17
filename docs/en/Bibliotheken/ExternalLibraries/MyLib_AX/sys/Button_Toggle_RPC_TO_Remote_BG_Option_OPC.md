# Button_Toggle_RPC_TO_Remote_BG_Option_OPC

![Button_Toggle_RPC_TO_Remote_BG_Option_OPC_network](./Button_Toggle_RPC_TO_Remote_BG_Option_OPC_network.svg)

* * * * * * * * * *

## Introduction

`Button_Toggle_RPC_TO_Remote_BG_Option_OPC` is an IO diagnostic button: on release it calls an RPC toggle method on the target module via `CLIENT_0`, and the background colour comes from a remote subscribe. The block corresponds to [`Button_Toggle_RPC_TO_Remote_BG_OPC`](./Button_Toggle_RPC_TO_Remote_BG_OPC.md), but is activated via `AOption` instead of `QI`/`INIT`. `AOption_OUT` passes the activation on after its own `INIT` (daisy chain like `OptionalGroup_001a`). It is a copy of `Button_Toggle_RPC_TO_Remote_BG_OPC` in which `INIT`/`INITO`/`QI`/`QO` were replaced by `AOption_IN` and `AOption_OUT`.

![Button_Toggle_RPC_TO_Remote_BG_Option_OPC](Button_Toggle_RPC_TO_Remote_BG_Option_OPC.svg)

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

| Variable | Type | Default | Comment |
|----------|------|---------|---------|
| `u16ObjId` | UINT | `ID_NULL` | Object ID button/background. |
| `ID_TRIGGER_CALL` | WSTRING |  | Remote method address (`ACTION=CALL_METHOD`) for the argument-less trigger method call on the target module. |
| `ID_STATE_READ` | WSTRING |  | Locally monitored address (`BOOL`, `ACTION=READ`) for the flip-flop state; written remotely by the target module. |

### **Data Outputs**

None.

### **Adapters**

| Adapter | Type | Direction | Comment |
|---------|------|-----------|---------|
| `AOption_IN` | `Option::AOption` | Socket | Drives `INIT`/`QI`; connect to `AOption_AUI_DEMUX_N.OUTx` or the `AOption_OUT` of an upstream instance. |
| `AOption_OUT` | `Option::AOption` | Plug | Passes the activation on to the next link after its own INIT (daisy chain); must be connected, end of chain e.g. `DummyGroup`. |

## Function Blocks (FBs) Used

- **AOption_X_TO_EVENT** (`Option::conversion::AOption_X_TO_EVENT`): converts the option into `INIT_REQ`/`QI_OUT` and accepts `INIT_CNF`
- **Button_Toggle** (`isobus::UT::io::Button::Button_IE`, `InputEvent = BT_RELEASED_UNLATCHED`): VT button, event on release
- **TRIGGER_CLIENT** (`iec61499::net::CLIENT_0`): calls the method on the target module
- **STATE_SUBSCRIBE** (`adapter::net::AX_SUBSCRIBE_1`): remote subscribe of the state
- **GreenWhiteBackground_AX** (SubApp `MyLib::sys::GreenWhiteBackground1_AX`): background colour; see [`GreenWhiteBackground1_AX`](./GreenWhiteBackground1_AX.md)
- **AOption_EVENT_TO_X** (`Option::conversion::AOption_EVENT_TO_X`): passes the activation on to `AOption_OUT`

## Program Flow and Connections

1. Initialization chain: `AOption_X_TO_EVENT.INIT_REQ` -> `Button_Toggle.INIT`; `Button_Toggle.INITO` -> `TRIGGER_CLIENT.INIT`; `TRIGGER_CLIENT.INITO` -> `STATE_SUBSCRIBE.INIT`; `STATE_SUBSCRIBE.INITO` -> `GreenWhiteBackground_AX.INIT`; `GreenWhiteBackground_AX.INITO` -> `AOption_EVENT_TO_X.INIT_REQ`; `AOption_EVENT_TO_X.INIT_CNF` -> `AOption_X_TO_EVENT.INIT_CNF`.
2. Method call: `Button_Toggle.IND` -> `TRIGGER_CLIENT.REQ`.
3. Qualifier: `AOption_X_TO_EVENT.QI_OUT` -> `Button_Toggle.QI`, `TRIGGER_CLIENT.QI`, `STATE_SUBSCRIBE.QI`, `GreenWhiteBackground_AX.QI` and `AOption_EVENT_TO_X.QI_OUT`.
4. Option: `AOption_IN` -> `AOption_X_TO_EVENT.AOption_IN`; `AOption_EVENT_TO_X.AOption_OUT` -> `AOption_OUT`.
5. Status: `STATE_SUBSCRIBE.OUT` -> `GreenWhiteBackground_AX.DI1`.
6. Data: `u16ObjId` -> `Button_Toggle.u16ObjId` and `GreenWhiteBackground_AX.u16ObjId`; `ID_TRIGGER_CALL` -> `TRIGGER_CLIENT.ID`; `ID_STATE_READ` -> `STATE_SUBSCRIBE.ID`.

## Technical Features

- The block has no event or `QI`/`QO` connections of its own; activation and acknowledgment run exclusively via `AOption_IN`/`AOption_OUT`.
- `AOption_OUT` must be connected onward (e.g. `DummyGroup` as end of chain), otherwise no `INIT_CNF` is returned.
- The state is not computed here but delivered remotely by the target module.

## Application Scenarios

- Optional diagnostic button that triggers an output toggle on a remote module and shows its state in colour, active only when the option is selected.
- Several instances sharing the same option, chained via `AOption_OUT`.

## Summary

`Button_Toggle_RPC_TO_Remote_BG_Option_OPC` triggers a remote method call on release, shows the remotely delivered state as background colour and is activated and chained via `AOption`.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

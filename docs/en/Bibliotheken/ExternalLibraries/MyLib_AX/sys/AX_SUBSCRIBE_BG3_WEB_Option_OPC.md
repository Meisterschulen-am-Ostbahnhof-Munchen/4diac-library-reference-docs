# AX_SUBSCRIBE_BG3_WEB_Option_OPC

![AX_SUBSCRIBE_BG3_WEB_Option_OPC_network](./AX_SUBSCRIBE_BG3_WEB_Option_OPC_network.svg)

* * * * * * * * * *

## Introduction

`AX_SUBSCRIBE_BG3_WEB_Option_OPC` is the option-lib variant of [`AX_SUBSCRIBE_BG3_WEB_OPC`](./AX_SUBSCRIBE_BG3_WEB_OPC.md): OPC-UA remote subscribe with VT background colour on SoftKey AND AuxFunction2 plus local web republish. The blocks with a real `QI` (`AX_SUBSCRIBE_1` and `STATUS_WEB_PUBLISH`) are initialized and gated sequentially by the adapter socket [`Option::AOption`](../../Option/AOption.md). `GreenWhiteBackground3_AX` remains unchanged and is driven as in the original (see Technical Features). The plug `PLUG1` passes the activation on to a downstream group (daisy chain).

![AX_SUBSCRIBE_BG3_WEB_Option_OPC](AX_SUBSCRIBE_BG3_WEB_Option_OPC.svg)

## Interface Structure

### **Event Inputs**

| Event | Type | Comment |
|-------|------|---------|
| `INIT` | EInit | Initialization request (to `GreenWhiteBackground3_AX.INIT`). |

### **Event Outputs**

| Event | Type | Comment |
|-------|------|---------|
| `INITO` | EInit | Initialization confirm (from `GreenWhiteBackground3_AX.INITO`). |

### **Data Inputs**

| Variable | Type | Default | Comment |
|----------|------|---------|---------|
| `QI` | BOOL | `TRUE` | Quality input, goes to `GreenWhiteBackground3_AX.QI`. |
| `u16ObjId` | UINT | `ID_NULL` | Object ID SoftKey/background (VT). |
| `u16ObjIdA` | UINT | `ID_NULL` | Object ID AuxFunction2/background (VT). |
| `ID_SUBSCRIBE` | WSTRING | | Remote subscribe address (status/colour, `ACTION=SUBSCRIBE`). |
| `ID_STATUS_WEB` | WSTRING | | Local publish address for the state reported back by the target module, so that a web client (e.g. `vt-ui-mirror`) can show the same background colour as the real VT. |

### **Data Outputs**

| Variable | Type | Comment |
|----------|------|---------|
| `QO` | BOOL | Quality output (from `GreenWhiteBackground3_AX.QO`). |

### **Adapters**

| Adapter | Type | Direction | Comment |
|---------|------|-----------|---------|
| `OPTION` | `Option::AOption` | Socket | Drives `INIT`/`QI` of `AX_SUBSCRIBE_1` and `STATUS_WEB_PUBLISH`; typically fed by the `PLUG1` of [`Softkey_Aux_IXA_TO_Remote_WRITE_Option`](./Softkey_Aux_IXA_TO_Remote_WRITE_Option.md). |
| `PLUG1` | `Option::AOption` | Plug | Passes the activation on to a downstream group (daisy chain); leave unused if nothing follows. |

## Function Blocks (FBs) Used

### Sub-blocks: AX_SUBSCRIBE_BG3_WEB_Option_OPC

- **Type**: SubAppType
- **Internal FBs used**:
    - **AOption_X_TO_EVENT** (`Option::conversion::AOption_X_TO_EVENT`): converts the socket `OPTION` into `INIT_REQ`/`QI_OUT`, see [`AOption_X_TO_EVENT`](../../Option/conversion/AOption_X_TO_EVENT.md).
    - **AX_SUBSCRIBE_1** (`adapter::net::AX_SUBSCRIBE_1`): OPC-UA subscribe on `ID_SUBSCRIBE`.
    - **AX_SPLIT_2** (`adapter::events::unidirectional::AX_SPLIT_2`): distributes the subscribe value to two branches.
    - **GreenWhiteBackground3_AX** (SubApp `MyLib::sys::GreenWhiteBackground3_AX`): sets the background colour on SoftKey and AuxFunction2.
    - **STATUS_WEB_PUBLISH** (`adapter::net::AX_PUBLISH_1`): local publish of the status on `ID_STATUS_WEB`.
    - **AOption_EVENT_TO_X** (`Option::conversion::AOption_EVENT_TO_X`): passes the activation on to `PLUG1`.

## Program Flow and Connections

1. Option chain: `AOption_X_TO_EVENT.INIT_REQ` -> `AX_SUBSCRIBE_1.INIT`; `AX_SUBSCRIBE_1.INITO` -> `STATUS_WEB_PUBLISH.INIT`; `STATUS_WEB_PUBLISH.INITO` -> `AOption_EVENT_TO_X.INIT_REQ`; `AOption_EVENT_TO_X.INIT_CNF` -> `AOption_X_TO_EVENT.INIT_CNF`.
2. `AOption_X_TO_EVENT.QI_OUT` -> `AX_SUBSCRIBE_1.QI`, `STATUS_WEB_PUBLISH.QI` and `AOption_EVENT_TO_X.QI_OUT`.
3. Adapters: `OPTION` -> `AOption_X_TO_EVENT.AOption_IN`; `AOption_EVENT_TO_X.AOption_OUT` -> `PLUG1`.
4. Data: `AX_SUBSCRIBE_1.OUT` -> `AX_SPLIT_2.IN`; `AX_SPLIT_2.OUT1` -> `GreenWhiteBackground3_AX.DI1`; `AX_SPLIT_2.OUT2` -> `STATUS_WEB_PUBLISH.IN`.
5. Parameters: `ID_SUBSCRIBE` -> `AX_SUBSCRIBE_1.ID`; `ID_STATUS_WEB` -> `STATUS_WEB_PUBLISH.ID`; `u16ObjId`/`u16ObjIdA` -> `GreenWhiteBackground3_AX.u16ObjId`/`u16ObjIdA`.
6. Outside the option chain: `INIT` -> `GreenWhiteBackground3_AX.INIT`, `GreenWhiteBackground3_AX.INITO` -> `INITO`, `QI` -> `GreenWhiteBackground3_AX.QI`, `GreenWhiteBackground3_AX.QO` -> `QO`.

## Technical Features

- **Only QI blocks gated:** Only `AX_SUBSCRIBE_1` and `STATUS_WEB_PUBLISH` (both have `QI`) hang on the option socket. `GreenWhiteBackground3_AX` has no `QI`/`QO` gating via the option; activation there works through `u16ObjId` versus `ID_NULL` in its own INIT, and without an active subscription in front of it, it is not driven with fresh data anyway.
- **Option initialization separate from `INIT`/`INITO`:** The input `INIT` and the output `INITO` belong to `GreenWhiteBackground3_AX`; the option chain has its own `INIT_REQ`/`INIT_CNF` handshake.
- **Daisy chain:** `PLUG1` allows further instances or groups to be attached to the same option.
- The counterpart without option is [`AX_SUBSCRIBE_BG3_WEB_OPC`](./AX_SUBSCRIBE_BG3_WEB_OPC.md).

## Application Scenarios

- Status half of [`Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC`](./Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC.md): background colour on SoftKey and Aux from a remote subscribe, active only when the option is selected.

## Summary

`AX_SUBSCRIBE_BG3_WEB_Option_OPC` combines remote subscribe, background colour on SoftKey and Aux and local web republish, and gates the blocks with `QI` via an `AOption` socket with forwarding through `PLUG1`.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

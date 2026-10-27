# AE_AX_SERVER_0_PUBLISH_1

![AE_AX_SERVER_0_PUBLISH_1_network](./AE_AX_SERVER_0_PUBLISH_1_network.svg)

* * * * * * * * * *

## Introduction

`AE_AX_SERVER_0_PUBLISH_1` is the local variant of [`adapter::net::AE_AX_SERVER_0_CLIENT_1_0`](../../adapter/net/AE_AX_SERVER_0_CLIENT_1_0.md): instead of a remote `CLIENT_1_0`, a local `AX_PUBLISH_1` writes (single device). The block receives one OPC-UA method call via `SERVER_0` and publishes the resulting state locally; both sit behind a single bidirectional `AE_AX` adapter plug `TRIGGER`. Internally adapter converters (`AE_EVENT_TO_E`, `AE_TO_AE_AX_SPLIT`) and `AX_PUBLISH_1` are used, because a SubApp cannot use the pins of an interface adapter directly. The block is created as a SubAppType so that FORTE does not need to know the type (no ELF for MyLib_AX).

![AE_AX_SERVER_0_PUBLISH_1](AE_AX_SERVER_0_PUBLISH_1.svg)

## Interface Structure

### **Event Inputs**

- `INIT` (EInit): Initialization

### **Event Outputs**

- `INITO` (EInit): Initialization confirm
- `CNF` (Event): `QO`/`STATUS` updated

### **Data Inputs**

| Variable | Type | Comment |
|----------|-----|-----------|
| `QI` | BOOL | Qualifier for all internal network blocks |
| `ID` | WSTRING | Local method address (`ACTION=CREATE_METHOD`) for the Trigger method call (`SERVER_0_1`) |
| `ID_STATE` | WSTRING | Local address (`ACTION=WRITE`) for the state (`STATE_PUBLISH`) |

### **Data Outputs**

| Variable | Type | Comment |
|----------|-----|-----------|
| `QO` | BOOL | TRUE only if `SERVER_0_1` and `STATE_PUBLISH` currently all report `QO = TRUE` |
| `STATUS_TRIGGER` | WSTRING | `STATUS` of `SERVER_0_1` |
| `STATUS_STATE` | WSTRING | `STATUS` of `STATE_PUBLISH` |

### **Adapters**

| Adapter | Type | Direction | Comment |
|---------|-----|----------|-----------|
| `TRIGGER` | `adapter::types::bidirectional::AE_AX` | Plug | Trigger (in), state (out) |

## Function Blocks Used (FBs)

- **SERVER_0_1** (`iec61499::net::SERVER_0`): receives the Trigger method call (`IND`)
- **EVENTS** (`adapter::conversion::unidirectional::AE_EVENT_TO_E`): adapter converter, maps the `IND` events onto the adapter; see [AE_EVENT_TO_E](../../adapter/conversion/unidirectional/EVENT/AE_EVENT_TO_E.md)
- **SPLIT** (`adapter::events::bidirectional::AE_TO_AE_AX_SPLIT`): splits the adapter into `OUT` (to plug `TRIGGER`) and `AX_OUT` (to the state publish); see [AE_TO_AE_AX_SPLIT](../../adapter/events/bidirectional/AE_TO_AE_AX_SPLIT.md)
- **STATE_PUBLISH** (`adapter::net::AX_PUBLISH_1`): publishes the state locally; see [AX_PUBLISH_1](../../adapter/net/AX_PUBLISH_1.md)
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_2`): ANDs the `QO` values of all network blocks

## Program Flow and Connections

1. `INIT` -> `SERVER_0_1.INIT`; `SERVER_0_1.INITO` -> `STATE_PUBLISH.INIT`; `STATE_PUBLISH.INITO` -> `INITO` (sequential initialization).
2. `SERVER_0_1.IND` -> `SERVER_0_1.RSP` (self-loop directly on the server, see the rule below).
3. `SERVER_0_1.IND` -> `EVENTS.REQ`.
4. `EVENTS.AE_OUT` -> `SPLIT.IN`; `SPLIT.OUT` -> `TRIGGER`; `SPLIT.AX_OUT` -> `STATE_PUBLISH.IN`.
5. `QI` -> `SERVER_0_1.QI` and `STATE_PUBLISH.QI`.
6. `ID` -> `SERVER_0_1.ID`, `ID_STATE` -> `STATE_PUBLISH.ID`.
7. `SERVER_0_1.QO` -> `AND_QO.IN1`; `STATE_PUBLISH.QO` -> `AND_QO.IN2`; `AND_QO.OUT` -> `QO`.
8. `STATE_PUBLISH.INITO`, `SERVER_0_1.IND` and `STATE_PUBLISH.CNF` -> `AND_QO.REQ`; `AND_QO.CNF` -> `CNF`.
9. `SERVER_0_1.STATUS` -> `STATUS_TRIGGER`, `STATE_PUBLISH.STATUS` -> `STATUS_STATE`.

## Technical Features

- The adapter converters are required because a SubApp cannot use the pins of an interface adapter directly: the converter maps the `IND` events onto the adapter, and the split separates the plug part from the state path to `AX_PUBLISH_1`.
- Local variant: `AX_PUBLISH_1` instead of a remote `CLIENT_1_0` (single device).
- `QO` is TRUE only if all 2 network blocks report `QO = TRUE`; each indication and `STATE_PUBLISH.CNF` additionally trigger `CNF`.
- **Rule `IND` → `RSP`**: Every `SERVER_0` needs `IND` → `RSP` directly on its own block. Without `RSP`, FORTE waits in `onServerMethodCall` for up to 4 s and holds open62541's global service lock meanwhile. During that time **all** OPC-UA publishes of the station stall, not only the one of the method. The application's reaction hangs on a second, independent connection from the same `IND`; `RSP` is never delayed. See [SERVER_0](../../../StandardLibraries/net/SERVER_0.md).

## Application Scenarios

- Receiving OPC-UA method calls with local state publication on a single device, e.g. in local exercise setups.
- Replacement for the same-named `adapter::net` variant `AE_AX_SERVER_0_CLIENT_1_0` with remote `CLIENT_1_0` when two devices are involved.

## Comparison with Similar Blocks

- **[AE_AX_SERVER_0_CLIENT_1_0](../../adapter/net/AE_AX_SERVER_0_CLIENT_1_0.md)**: same interface, but remote `CLIENT_1_0` instead of a local `AX_PUBLISH_1`.

## Summary

`AE_AX_SERVER_0_PUBLISH_1` receives one method call via `SERVER_0` and publishes the state locally via `AX_PUBLISH_1`, both behind an `AE_AX` plug. It is the local variant of `AE_AX_SERVER_0_CLIENT_1_0`.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

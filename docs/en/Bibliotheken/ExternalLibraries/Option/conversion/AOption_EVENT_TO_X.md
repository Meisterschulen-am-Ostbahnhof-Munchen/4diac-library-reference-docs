# AOption_EVENT_TO_X

![AOption_EVENT_TO_X](./AOption_EVENT_TO_X.svg)

![AOption_EVENT_TO_X_network](./AOption_EVENT_TO_X_network.svg)

* * * * * * * * * *

## Introduction

`AOption_EVENT_TO_X` is a composite FB that maps events and data onto the option adapter [`AOption`](../AOption.md) (fan-out, holds the PLUG).

## Interface Structure

### **Event Inputs**

| Event | Type | Comment |
|-------|------|---------|
| `INIT_REQ` | EInit | Request to the adapter. Carries the data `QI_OUT`. |

### **Event Outputs**

| Event | Type | Comment |
|-------|------|---------|
| `INIT_CNF` | EInit | Confirmation from the adapter. |

### **Data Inputs**

| Variable | Type | Comment |
|----------|------|---------|
| `QI_OUT` | BOOL | Event input qualifier (IEC 61499-1): `TRUE` = request/activate option, `FALSE` = withdraw. |

### **Data Outputs**

None.

### **Adapters**

| Adapter | Type | Direction | Comment |
|---------|------|-----------|---------|
| `AOption_OUT` | `Option::AOption` | Plug | Adapter output. |

## Functionality

The internal network consists of connections only (no further FBs):

- `INIT_REQ` -> `AOption_OUT.INIT_REQ`
- `AOption_OUT.INIT_CNF` -> `INIT_CNF`
- `QI_OUT` -> `AOption_OUT.QI_OUT`

A request is thus passed on unchanged to the adapter, and the socket's confirmation returns as `INIT_CNF`.

## Technical Features

- Pure pass-through without logic.
- The counterpart is [`AOption_X_TO_EVENT`](./AOption_X_TO_EVENT.md), which holds the SOCKET.

## Application Scenarios

- A requester network sets an option at an option group via this block.

## Conclusion

`AOption_EVENT_TO_X` exposes the plug of the `AOption` adapter as an ordinary event/data interface (`INIT_REQ`, `QI_OUT`, `INIT_CNF`).

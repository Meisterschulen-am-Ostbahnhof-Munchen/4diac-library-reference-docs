# AOption_X_TO_EVENT

![AOption_X_TO_EVENT](./AOption_X_TO_EVENT.svg)

![AOption_X_TO_EVENT_network](./AOption_X_TO_EVENT_network.svg)

* * * * * * * * * *

## Introduction

`AOption_X_TO_EVENT` is a composite FB that converts the option adapter [`AOption`](../AOption.md) into events and data (fan-in, holds the SOCKET). It is used in option groups to receive and acknowledge the request (`INIT_REQ`, `QI_OUT`).

## Interface Structure

### **Event Inputs**

| Event | Type | Comment |
|-------|------|---------|
| `INIT_CNF` | EInit | Confirmation to the adapter. |

### **Event Outputs**

| Event | Type | Comment |
|-------|------|---------|
| `INIT_REQ` | EInit | Request from the adapter. Carries the data `QI_OUT`. |

### **Data Inputs**

None.

### **Data Outputs**

| Variable | Type | Comment |
|----------|------|---------|
| `QI_OUT` | BOOL | Event input qualifier (IEC 61499-1): `TRUE` = request/activate option, `FALSE` = withdraw. |

### **Adapters**

| Adapter | Type | Direction | Comment |
|---------|------|-----------|---------|
| `AOption_IN` | `Option::AOption` | Socket | Adapter input. |

## Functionality

The internal network consists of connections only (no further FBs):

- `AOption_IN.INIT_REQ` -> `INIT_REQ`
- `INIT_CNF` -> `AOption_IN.INIT_CNF`
- `AOption_IN.QI_OUT` -> `QI_OUT`

The plug's request appears as the event `INIT_REQ` with `QI_OUT`; once the group is initialized, `INIT_CNF` is fed into the block and reported back to the plug.

## Technical Features

- Pure pass-through without logic.
- The counterpart is [`AOption_EVENT_TO_X`](./AOption_EVENT_TO_X.md), which holds the PLUG.
- The SubApps [`OptionalGroup_001`](../../MyLib_AX_Option/sys/OptionalGroup_001.md) and [`DummyGroup`](../../MyLib_AX_Option/sys/DummyGroup.md) use it.

## Application Scenarios

- Option group: `QI_OUT` activates/deactivates the group, `INIT_CNF` acknowledges the activation.

## Conclusion

`AOption_X_TO_EVENT` exposes the socket of the `AOption` adapter as an ordinary event/data interface (`INIT_REQ`, `QI_OUT`, `INIT_CNF`).

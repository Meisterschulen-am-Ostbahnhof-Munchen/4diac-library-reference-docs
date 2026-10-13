# AOption

**Plug (adapter output)**

![AOption_plug](AOption_plug.svg)

**Socket (adapter input)**

![AOption_socket](AOption_socket.svg)

* * * * * * * * * *

## Introduction

The `AOption` adapter (library `Option`) is an option management adapter: it carries an INIT request/confirm handshake with the event input qualifier `QI` (IEC 61499-1). The plug side (requester) can request/activate (`QI=TRUE`) or withdraw/terminate (`QI=FALSE`) an option at the socket side (option group); the socket acknowledges with `INIT_CNF`.

## Interface Structure

### **Event Inputs**

| Event | Comment |
|-------|---------|
| `INIT_CNF` | Initialization confirmed (SOCKET -> PLUG). |

### **Event Outputs**

| Event | Comment |
|-------|---------|
| `INIT_REQ` | Request initialization (PLUG -> SOCKET). Carries the data `QI_OUT`. |

### **Data Inputs**

None.

### **Data Outputs**

| Variable | Type | Comment |
|----------|------|---------|
| `QI_OUT` | BOOL | Event input qualifier for `INIT_REQ` (IEC 61499-1): `TRUE` = request/activate option (INIT_REQ+), `FALSE` = withdraw/terminate (INIT_REQ-). |

(The directions are given from the plug side; they are mirrored on the socket side.)

## Functionality

The plug sends `INIT_REQ` together with `QI_OUT`. The socket evaluates `QI_OUT` (activate or deactivate the option group) and answers with `INIT_CNF` when done. The adapter itself contains no logic; it only defines the interface. To connect it in your own networks, use the converters [`AOption_EVENT_TO_X`](./conversion/AOption_EVENT_TO_X.md) (holds the plug) and [`AOption_X_TO_EVENT`](./conversion/AOption_X_TO_EVENT.md) (holds the socket).

## Technical Features

- **Handshake**: every request is acknowledged with `INIT_CNF` (request/confirm).
- **Data binding**: `QI_OUT` is bound to the event `INIT_REQ` via `With`.
- **No state machine**: the adapter is a pure interface type.

## Application Scenarios

- Switching optional function groups (e.g. groups containing I/O blocks) on and off via a selection block such as [`AOption_AD_DEMUX_2`](./selection/AOption_AD_DEMUX_2.md).

## Conclusion

`AOption` provides a lean INIT_REQ/INIT_CNF handshake with `QI` for activating and deactivating optional parts of an application.

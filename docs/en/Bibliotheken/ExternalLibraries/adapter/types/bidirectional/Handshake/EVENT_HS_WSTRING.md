# EVENT_HS_WSTRING

![EVENT_HS_WSTRING](./EVENT_HS_WSTRING.svg)

* * * * * * * * * *

## Introduction

The **EVENT_HS_WSTRING** adapter is a data-carrying variant of the classic **Handshake design pattern** (IEC 61499 primer course, Module 6 – Design methods and patterns). It maintains the same REQ/CNF/IND/RSP event vocabulary as the dataless `EVENT_HS` adapter, but each event is accompanied by a `WSTRING` payload. This matches Vyatkin's generic *service* adapter (slide 48) used in service-oriented architectures, where messages such as `"push,100"` are exchanged.

The adapter implements a **plug/socket** role split:

- **Plug** (client/requester side) fires REQ and RSP events and reacts to CNF and IND events.
- **Socket** (server/responder side) fires CNF and IND events and reacts to REQ and RSP events.

## Interface Structure

### **Event Inputs**

| Event | With Variable | Description |
|-------|---------------|-------------|
| `CNF` | `CNFD` | Confirmation from Socket to Plug, answers a REQ. Carries a payload via `CNFD`. |
| `IND` | `INDD` | Unsolicited indication from Socket to Plug. Carries a payload via `INDD`. |

### **Event Outputs**

| Event | With Variable | Description |
|-------|---------------|-------------|
| `REQ` | `REQD` | Request from Plug to Socket. Carries a payload via `REQD`. |
| `RSP` | `RSPD` | Response from Plug to Socket, answers an IND. Carries a payload via `RSPD`. |

### **Data Inputs**

| Variable | Type | Description |
|----------|------|-------------|
| `CNFD` | `WSTRING` | Confirmation payload, accompanies `CNF`. |
| `INDD` | `WSTRING` | Indication payload, accompanies `IND`. |

### **Data Outputs**

| Variable | Type | Description |
|----------|------|-------------|
| `REQD` | `WSTRING` | Request payload, accompanies `REQ`. |
| `RSPD` | `WSTRING` | Response payload, accompanies `RSP`. |

### **Adapters**

The adapter itself acts as an interface between a Plug and a Socket. It does not contain any sub-adapters; instead, it defines the contract for bidirectional communication with WSTRING payloads.

## Functionality

The adapter follows two primary interaction patterns:

1. **Request–Confirm** (`request_confirm`):
   - The Plug issues a `REQ` event with a payload (`REQD`).
   - The Socket receives the `REQ` and later responds with a `CNF` event carrying `CNFD`.

2. **Indication–Response** (`indication_response`):
   - The Socket sends an unsolicited `IND` event with payload (`INDD`).
   - The Plug acknowledges with an `RSP` event carrying `RSPD`.

Each transaction is atomic and unidirectional from the initiating side. The WSTRING payloads allow arbitrary textual data to be exchanged, making the adapter suitable for command/status or simple data transfer scenarios.

## Technical Features

- **Standard compliance**: Implements the IEC 61499 service interface pattern.
- **Data type**: All payload variables are of type `WSTRING`, allowing Unicode string data.
- **Explicit event–data association**: Each event has a corresponding data variable, ensuring payload consistency.
- **Socket/Plug semantics**: Clearly defined roles for client and server sides.
- **Reusability**: Can be used as a template for typed payload adapters by replacing `WSTRING` with other types.
- **Documentation**: Detailed usage notes are embedded in the type's documentation attribute, referencing typical message formats like `"push,100"`.

## State Overview

The adapter does not maintain an internal state machine; it defines a stateless protocol with two independent service sequences. However, each sequence can be considered a two-step exchange:

- **Idle** → `REQ` sent → waiting for `CNF` → `CNF` received → back to idle.
- **Idle** → `IND` received → `RSP` sent → back to idle.

These sequences can overlap or be interleaved, as the adapter does not enforce mutual exclusion.

## Application Scenarios

- **Service-oriented architectures** in distributed control systems, where a client requests an operation and receives confirmation.
- **Plant/process data interfaces** where a server sends unsolicited indications (e.g., alarms) and the client acknowledges.
- **Command/response patterns**: e.g., sending `"push,100"` to a machine and receiving `"ok"` as confirmation.
- **Replacement for dataless handshake** when payload transmission is required without redesigning the event structure.

## Comparison with Similar Blocks

| Adapter | Payload | Use Case |
|---------|---------|----------|
| `EVENT_HS` | None | Pure handshake pattern, no data transfer. |
| `EVENT_HS_WSTRING` | `WSTRING` | Handshake with textual payload, generic and human-readable. |
| Typed adapters (e.g., with `INT`/`REAL`) | specific types | When strict typing and performance are critical; require custom design. |

Compared to `EVENT_HS`, this adapter adds data transfer capability while preserving the same event flow. Compared to fully typed adapters, it offers flexibility and simplicity for heterogeneous string-based data.

## Conclusion

The `EVENT_HS_WSTRING` adapter extends the proven handshake pattern with WSTRING payloads, enabling clear, bidirectional, and data-bearing communication between plug and socket components. It is particularly useful in service-oriented industrial applications where requests, confirmations, indications, and responses need to carry textual information. Its clear separation of events and data, combined with the standard plug/socket roles, makes it a versatile building block for IEC 61499-based systems.

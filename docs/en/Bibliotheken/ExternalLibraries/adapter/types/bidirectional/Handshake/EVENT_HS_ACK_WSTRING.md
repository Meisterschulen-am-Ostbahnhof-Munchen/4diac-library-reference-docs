# EVENT_HS_ACK_WSTRING

![EVENT_HS_ACK_WSTRING](./EVENT_HS_ACK_WSTRING.svg)

* * * * * * * * * *

## Introduction

`EVENT_HS_ACK_WSTRING` is a data-carrying adapter type belonging to the Handshake (`EVENT_HS`) family defined in the IEC 61499 primer course (Module 6, Valeriy Vyatkin). It implements the request/confirm half of the full REQ/CNF/IND/RSP handshake vocabulary, augmented with `WSTRING` payloads on both events.

This adapter provides a simple **Plug → Socket request → confirmation** pattern, where the Plug (client/requester) initiates a request with an attached payload, and the Socket (server/responder) answers with a confirmation carrying its own payload. Notably, the Socket cannot initiate unsolicited traffic; no `IND`/`RSP` events exist, making this a strictly client-driven, two-message exchange.

## Interface Structure

The adapter exposes two complementary roles — **Plug** (left interface, `Name>>`) and **Socket** (right interface, `>>Name`) — which must be paired by connecting a Plug of one instance to a Socket of another.

### **Event Inputs**

| Event | With | Comment |
|-------|------|---------|
| `CNF` | `CNFD` | Confirmation from Socket to Plug, answers a `REQ` |

### **Event Outputs**

| Event | With | Comment |
|-------|------|---------|
| `REQ` | `REQD` | Request from Plug to Socket |

### **Data Inputs**

| Data | Type | Comment |
|------|------|---------|
| `CNFD` | `WSTRING` | Confirmation payload, accompanies `CNF` |

### **Data Outputs**

| Data | Type | Comment |
|------|------|---------|
| `REQD` | `WSTRING` | Request payload, accompanies `REQ` |

### **Adapters**

The adapter defines two interface halves that act as the connection endpoints:

- **PLUG** (`Name>>`) — the request-initiating side. It fires `REQ`/`REQD` and receives `CNF`/`CNFD`.
- **SOCKET** (`>>Name`) — the request-responding side. It receives `REQ`/`REQD` and fires `CNF`/`CNFD`.

## Functionality

The adapter implements a synchronous-style request/confirm handshake:

1. The **Plug** issues a request by firing the `REQ` event, simultaneously placing a value on `REQD` (e.g., `"push,100"`).
2. The **Socket** receives the `REQ` event with `REQD` and processes the request.
3. Upon completion, the **Socket** responds by firing `CNF`, placing its own payload on `CNFD`.
4. The **Plug** receives the `CNF` event with `CNFD` and can react to the confirmation.

The entire exchange is captured in the service sequence `request_confirm`, which defines two mandatory transactions: one for the REQ/REQD transfer from Plug to Socket, and one for the CNF/CNFD transfer from Socket back to Plug.

## Technical Features

- **WSTRING payloads** on both `REQ` and `CNF` events, allowing arbitrary textual data (e.g., `"name,value"` messages) to travel with each handshake step.
- **Reduced vocabulary** — only `REQ`/`CNF`; no `IND`/`RSP` events are present, preventing unsolicited Socket-initiated messages.
- **Direction-preserving roles** — the Plug always acts as requester, the Socket always as responder.
- **IEC 61499-1 compliant** — defined as an adapter type with standard event/data `With` associations.
- **Single service sequence** — the `request_confirm` sequence strictly defines the allowed interaction order.

## State Overview

The adapter does not maintain an explicit internal state machine in the FB sense; the interaction is governed by the service sequence `request_confirm`. Conceptually, the exchange progresses through two states:

| State | Description |
|-------|-------------|
| **Idle** | No pending request. The Plug may initiate a `REQ`/`REQD`. |
| **Waiting for Confirmation** | `REQ`/`REQD` has been sent; the adapter waits for the Socket's `CNF`/`CNFD`. |

After the `CNF`/`CNFD` is received, the adapter returns to Idle and is ready for the next request. The Socket side mirrors this: it waits in Idle for an incoming `REQ`, processes it, and returns to Idle after firing `CNF`.

## Application Scenarios

- **Client–Server command/acknowledge** patterns, where a controller (Plug) sends a command string and expects a textual acknowledgment (e.g., `"push,100"` → `"push,100"`).
- **Configuration or parameter update** workflows requiring a paired request/confirmation with meaningful data on both sides.
- **Reduced handshake requirements** in embedded or automation systems where only a two-message exchange is needed and the server must never push unsolicited data.
- **Inter-module communication** in distributed IEC 61499 applications where a simple data-carrying request–response interaction must be explicitly typed and verified at design time.

## Comparison with Similar Blocks

| Adapter | Events | Payload | Notes |
|---------|--------|---------|-------|
| `EVENT_HS` | REQ, CNF, IND, RSP | None | Full four-event handshake |
| `EVENT_HS_ACK` | REQ, CNF | None | Reduced two-event handshake |
| `EVENT_HS_UNI` | REQ, CNF | None | Unidirectional variant |
| `EVENT_HS_UNI_WSTRING` | REQ, CNF | WSTRING on one direction | Data-carrying unidirectional variant |
| `EVENT_HS_ACK_WSTRING` | REQ, CNF | WSTRING on both REQ and CNF | Data-carrying reduced handshake |

Compared to `EVENT_HS_ACK`, this adapter adds `WSTRING` payloads to both the request and the confirmation, making it suitable for message-based protocols. Compared to the full `EVENT_HS`, it drops the `IND`/`RSP` pair entirely, which simplifies the contract and eliminates the possibility of server-initiated traffic.

## Conclusion

`EVENT_HS_ACK_WSTRING` is a well-defined, minimal handshake adapter for request/confirm interactions with textual payloads. Its clean separation of Plug (requester) and Socket (responder) roles, combined with its reduced event vocabulary and explicit `WSTRING` data carriage, makes it a practical building block for IEC 61499 applications that need a simple, typed, two-message client–server exchange without the overhead or ambiguity of a full four-event handshake.

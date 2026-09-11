# EVENT_HS

![EVENT_HS](./EVENT_HS.svg)

* * * * * * * * * *
## Introduction

The EVENT_HS adapter interface implements the **Handshake design pattern** from the IEC 61499 primer course (Module 6 – Design methods and patterns, category *Behavioural*). It bundles the four classical service primitives of a request/confirm/indication/response interaction into a single adapter connection, eliminating the need for four separate event connections between communication partners. This is the minimal, dataless variant of the pattern – no payload variables are declared.

The adapter distinguishes two roles: the **Plug** plays the *Requester/client* role, firing `REQ` and `RSP` events and reacting to `CNF` and `IND` events. The **Socket** plays the *Responder/server* role, reacting to `REQ` and `RSP` and firing `CNF` and `IND` events.

## Interface Structure

### **Event Inputs**

| Name | Type | Comment |
|------|------|---------|
| CNF | Event | Confirmation from Socket to Plug, answers a REQ |
| IND | Event | Indication from Socket to Plug |

### **Event Outputs**

| Name | Type | Comment |
|------|------|---------|
| REQ | Event | Request from Plug to Socket |
| RSP | Event | Response from Plug to Socket, answers an IND |

### **Data Inputs**

None – this adapter is dataless by design.

### **Data Outputs**

None – this adapter is dataless by design.

### **Adapters**

This is itself an adapter type. It exposes a **Socket** interface on the right side and a **Plug** interface on the left side, allowing it to be connected as a bidirectional handshake channel between two function blocks.

## Functionality

The EVENT_HS adapter encapsulates two distinct service sequences:

1. **request_confirm** – A typical request cycle:
   - The Plug issues a `REQ` event, which is forwarded to the Socket.
   - The Socket processes the request and answers with a `CNF` event, which is forwarded back to the Plug.

2. **indication_response** – An unsolicited notification cycle:
   - The Socket emits an `IND` event, which is forwarded to the Plug.
   - The Plug acknowledges the indication with an `RSP` event, which is forwarded back to the Socket.

These two sequences can occur independently and asynchronously. The adapter itself does not enforce strict alternation between the two sequences; it merely provides the connection structure and the service protocol skeleton.

## Technical Features

- **Dataless design** – No data variables are involved; the adapter is purely event-driven, making it lightweight and easy to integrate.
- **Role-based directionality** – The Plug and Socket roles are clearly defined: Plug = Requester, Socket = Responder. This yields a natural left-to-right layout in the FBNetwork editor.
- **Service sequence documentation** – The two service sequences (`request_confirm` and `indication_response`) are formally declared in the adapter's service model, enabling validation and traceability.
- **Compiler compatibility** – Confirmed to work with the 4diac ECC compiler. Note that the compiler only checks that `HS.<Name>` refers to a declared event on the adapter; it does *not* verify that the direction of the event makes sense at the given Socket/Plug side. Correct wiring must therefore be ensured by the developer.

## State Overview

The EVENT_HS adapter does not maintain internal state. Its behaviour is defined entirely by the two independent service sequences. Each sequence moves through the following phases:

**request_confirm sequence:**
1. Idle – awaiting a `REQ` from the Plug.
2. Request sent – `REQ` has been forwarded to the Socket; awaiting `CNF`.
3. Confirmation received – `CNF` has been forwarded back to the Plug; sequence returns to Idle.

**indication_response sequence:**
1. Idle – awaiting an `IND` from the Socket.
2. Indication sent – `IND` has been forwarded to the Plug; awaiting `RSP`.
3. Response received – `RSP` has been forwarded back to the Socket; sequence returns to Idle.

The two sequences can overlap without interference, as they use disjoint event pairs.

## Application Scenarios

The EVENT_HS adapter is suitable for any scenario where two function blocks need a structured, handshake-based event communication, including:

- **Client/Server coordination** – A client FB requests an operation from a server FB and waits for confirmation of completion.
- **Asynchronous notifications** – A producer FB notifies a consumer FB of an event, and the consumer acknowledges receipt.
- **Resource management** – Coordinating access to shared resources between FBs, where explicit request/confirm cycles help avoid race conditions.
- **Simplified wiring** – Replacing multiple point-to-point event connections with a single adapter connection, improving diagram readability and maintainability.

## Comparison with Similar Blocks

| Aspect | EVENT_HS adapter | Plain event connections | Data-carrying handshake adapters |
|--------|------------------|------------------------|----------------------------------|
| Event bundling | Yes – 4 events in one adapter | No – separate wires per event | Yes |
| Payload support | None (dataless) | N/A | Yes – carries data with events |
| Role clarity | Explicit Plug/Socket roles | No implicit roles | Depends on implementation |
| Wiring complexity | Low – one connection | High – multiple wires | Low – one connection but more complex interface |
| Suitable for | Simple synchronization, pure event protocols | Simple one-way triggers | Data exchange with handshake |

The key advantage over plain event connections is the encapsulation of the complete handshake protocol into a single, well-defined interface. Compared to data-carrying handshake adapters, EVENT_HS is minimal and focused solely on event synchronization.

## Conclusion

The EVENT_HS adapter provides a clean, dataless implementation of the IEC 61499 handshake design pattern. By defining the four core service primitives (`REQ`, `CNF`, `IND`, `RSP`) within a single adapter interface, it simplifies the wiring between communication partners and clarifies the roles of Requester and Responder. Its lightweight nature and identical behaviour to the textbook pattern make it an ideal building block for event-driven coordination in IEC 61499-based applications. Developers must, however, pay careful attention to correct Socket/Plug assignment, since the compiler does not enforce directional correctness.
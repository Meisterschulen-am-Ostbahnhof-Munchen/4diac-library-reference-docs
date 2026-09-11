# TokenRing

![TokenRing](./TokenRing.svg)

* * * * * * * * * *

## Introduction

The **TokenRing** adapter type implements the **TokenRing / Mutual Exclusion design pattern** (IEC 61499 primer course, Module 6, V. Vyatkin, slide 15). It provides a dataless, event-based interface used to pass a mutual-exclusion "token" around a ring of controllers that share a common resource. Only the controller currently holding the token may enter its critical section; when its work is complete, it passes the token to its downstream neighbour.

The adapter defines exactly two events: **GIVE** (handing the token to the neighbour) and **RCV** (acknowledging receipt of the token). The adapter itself carries no data—the token is purely logical and conveyed by the event exchange.

## Interface Structure

The TokenRing adapter exposes two event endpoints, one on each side of the adapter connection:

- **Plug** side (left, named `MTXOUT` in the reference design) plays the **giver** role: it fires `GIVE` and reacts to `RCV`.
- **Socket** side (right, named `MTXIN` in the reference design) plays the **receiver** role: it reacts to `GIVE` and fires `RCV`.

### **Event Inputs**

| Event | Type | Description |
|-------|------|-------------|
| `RCV` | Event | Acknowledgement from the Socket that it received the token, sent back to the Plug. |

### **Event Outputs**

| Event | Type | Description |
|-------|------|-------------|
| `GIVE` | Event | The Plug hands the token to the Socket. |

### **Data Inputs**

None. The adapter is **dataless** by design—the token is represented solely by the event exchange.

### **Data Outputs**

None. No data is carried across the adapter.

### **Adapters**

This type is itself an adapter and does not contain nested adapters. It is instantiated as an adapter connection point inside function blocks or other adapters.

## Functionality

The adapter implements a strict two-step handshake per token pass, captured in the service sequence `token_pass`:

1. **Token handover**: The Plug fires `GIVE`; the event is forwarded to the Socket, signalling that the token is being passed to the downstream neighbour.
2. **Acknowledgement**: The Socket fires `RCV`; the event is forwarded back to the Plug, confirming that the token has been received.

This handshake ensures a deterministic, lossless token transfer. A controller that participates in the ring must instantiate **two** TokenRing adapters:

- One **Plug** (`MTXOUT`) connected towards its *downstream* neighbour (the controller it passes the token to).
- One **Socket** (`MTXIN`) connected towards its *upstream* neighbour (the controller it receives the token from).

The ring is formed by wiring `MTXOUT` of each controller to `MTXIN` of the next controller in the ring, creating a closed loop.

## Technical Features

- **Dataless design**: No data inputs or outputs; the token is purely logical, reducing interface complexity and eliminating data consistency issues.
- **Bidirectional event flow**: The adapter defines events travelling in both directions (Plug→Socket for `GIVE`, Socket→Plug for `RCV`), making it a true bidirectional adapter.
- **Plug/Socket role separation**: The roles are explicit and asymmetric, clearly identifying which side initiates the token pass and which side acknowledges it.
- **Standard-compliant**: Conforms to IEC 61499-1 adapter type definitions, with a formally declared service sequence (`token_pass`) that defines the valid event ordering.
- **Compact footprint**: Only two events, no data, no nested adapters—minimal resource usage in embedded controllers.

## State Overview

Since the adapter is dataless, its state is expressed through the progress of the handshake:

| State | Description |
|-------|-------------|
| **Idle (Token Held)** | No event pending. The token is held by the local controller (or the controller is waiting for the token from upstream). |
| **Token Being Passed** | The Plug has fired `GIVE`; the token is in transit to the downstream neighbour. The adapter waits for the `RCV` acknowledgement. |
| **Token Acknowledged** | The Socket has fired `RCV`; the handshake is complete. The adapter returns to the Idle state, ready for the next token pass. |

The service sequence enforces that `GIVE` must precede `RCV` in each token-pass cycle; no other event order is permitted.

## Application Scenarios

- **Mutually exclusive access to a shared resource**: Multiple controllers sharing a common device (e.g., a memory bank, an I/O module, or a communication link) coordinate via the token so that only the token holder accesses the resource at any time.
- **Distributed control loops**: In agricultural machinery, industrial automation, or process control, where several PLCs or embedded controllers must serialize access to a physical actuator or sensor.
- **Ring topologies**: Closed-loop networks of controllers where deterministic, fair token rotation is required—each controller gets the token in turn, guaranteeing bounded latency for resource access.
- **Safety-critical coordination**: Where a simple, verifiable protocol is needed to prevent simultaneous access to a critical resource, the dataless handshake minimizes failure modes.

## Comparison with Similar Blocks

| Feature | TokenRing Adapter | Conventional Binary Semaphore / Mutex FB | Simple Event Connection |
|---------|-------------------|------------------------------------------|-------------------------|
| Data payload | None (pure event) | Often carries ownership/ID data | None |
| Acknowledgment | Explicit `RCV` handshake | Implicit release | None |
| Directionality | Bidirectional (Plug→Socket and Socket→Plug) | Usually unidirectional request/grant | Unidirectional |
| Ring topology support | Native (two instances per node) | Requires additional logic for ring wiring | Not designed for this |
| Failure detection | Handshake ensures delivery; no ack means no token loss | Depends on FB implementation | No delivery guarantee |

Compared to a unidirectional event connection (which merely forwards a trigger), the TokenRing adapter provides a **confirmed handshake** that guarantees the token transfer is completed only when the receiver acknowledges it. Compared to a full semaphore FB, it avoids all data overhead and protocol complexity while still providing deterministic mutual exclusion.

## Conclusion

The TokenRing adapter is a minimal, robust, and standard-compliant solution for implementing mutual exclusion in ring-shaped distributed control systems. Its dataless design and strict two-event handshake make it easy to use, easy to verify, and well suited for embedded controllers where determinism and low overhead are critical. By instantiating one Plug and one Socket per controller, developers can build a closed token-passing ring that provides fair, bounded-latency access to shared resources across the entire network.

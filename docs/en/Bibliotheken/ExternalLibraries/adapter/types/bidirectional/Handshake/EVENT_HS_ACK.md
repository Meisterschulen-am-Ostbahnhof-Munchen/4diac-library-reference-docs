# EVENT_HS_ACK

![EVENT_HS_ACK](./EVENT_HS_ACK.svg)

* * * * * * * * * *
## Introduction
The EVENT_HS_ACK adapter is a reduced member of the EVENT_HS (Handshake) design-pattern family. It implements only the request/confirm half of the full handshake vocabulary, providing the **REQ** (request) and **CNF** (confirmation) events while omitting **IND** and **RSP**. This yields a strict, one-directional-per-transaction handshake in which every request is guaranteed to receive a confirmation.

Because the adapter is dataless, it is used purely for event synchronization and control-flow coordination between the Plug and Socket sides of a connection.

## Interface Structure
The adapter exposes exactly two event interfaces and no data interfaces. It defines two roles – **PLUG** (left interface) and **SOCKET** (right interface) – which determine in which direction the events travel.

### **Event Inputs**
| Event | Description |
|-------|-------------|
| **CNF** | Confirmation received from the Socket, answering a previously issued REQ. |

### **Event Outputs**
| Event | Description |
|-------|-------------|
| **REQ** | Request issued by the Plug towards the Socket. |

### **Data Inputs**
None – the adapter carries no data.

### **Data Outputs**
None – the adapter carries no data.

### **Adapters**
| Role | Direction | Behaviour |
|------|-----------|-----------|
| **PLUG** | Left interface | Plays the Requester/client role. It fires **REQ** and reacts to **CNF**. |
| **SOCKET** | Right interface | Plays the Responder/server role. It reacts to **REQ** and fires **CNF**. |

## Functionality
EVENT_HS_ACK implements a strict request/confirm handshake sequence:

1. The **Plug** issues a **REQ** event.
2. The **Socket** receives the **REQ**, performs whatever processing is required, and responds with a **CNF** event.
3. The **Plug** receives the **CNF**, which completes the transaction.

The Socket side can never push an unsolicited indication back to the Plug. Every **REQ** must be answered by exactly one **CNF**, making the pattern a reliable request/acknowledgment protocol.

## Technical Features
- **Dataless** – carries no data payload; it only synchronizes events.
- **Reduced vocabulary** – only REQ and CNF; no IND or RSP.
- **Guaranteed confirmation** – every REQ is answered by a CNF, unlike unidirectional handshake variants.
- **Standard IEC 61499 compliant** – adheres to the 61499-1 identification and uses the standard service-sequence model.
- **Simple service model** – a single `request_confirm` service sequence with two transactions fully specifies the behaviour.

## State Overview
The adapter defines one service sequence, `request_confirm`, consisting of two transactions:

| Transaction | Input Primitive | Output Primitive | Description |
|-------------|-----------------|------------------|-------------|
| 1           | PLUG **REQ**    | SOCKET **REQ**   | Plug sends a request to the Socket. |
| 2           | SOCKET **CNF**  | PLUG **CNF**     | Socket confirms the request back to the Plug. |

The handshake is strictly alternating: the Plug initiates, the Socket confirms, and the transaction is then complete.

## Application Scenarios
- **Command/acknowledgment patterns** – when a client needs to issue commands and wait for explicit confirmation that the command was executed.
- **Simple server interactions** – when the Socket side never needs to send unsolicited indications back to the Plug.
- **Control-system coordination** – useful in distributed automation where asynchronous requests must be reliably acknowledged.
- **Reduced communication overhead** – when only two event types are needed and carrying data is unnecessary.

## Comparison with Similar Blocks
| Block | REQ | CNF | IND | RSP | Data | Description |
|-------|-----|-----|-----|-----|------|-------------|
| **EVENT_HS** | ✓ | ✓ | ✓ | ✓ | – | Full handshake with all four event types. |
| **EVENT_HS_UNI** | ✓ | – | – | – | – | One-directional handshake without any confirmation. |
| **EVENT_HS_ACK** | ✓ | ✓ | – | – | – | Request/confirm only, no unsolicited indications. |
| **EVENT_HS_ACK_WSTRING** | ✓ | ✓ | – | – | ✓ | Same as EVENT_HS_ACK but carries a string payload. |

EVENT_HS_ACK differs from **EVENT_HS_UNI** in that it guarantees a confirmation for every request. It differs from the full **EVENT_HS** by removing the IND/RSP pair, which enforces a clean server/responder model where the Socket can only answer requests.

## Conclusion
EVENT_HS_ACK is a focused, dataless handshake adapter that provides a reliable request/confirm interaction pattern. By eliminating IND and RSP, it enforces a disciplined client/server relationship where every request is answered by a confirmation. It is an excellent choice for simple command/acknowledgment flows in IEC 61499 distributed control applications, offering a reduced interface footprint while preserving the essential guarantees of a handshake.
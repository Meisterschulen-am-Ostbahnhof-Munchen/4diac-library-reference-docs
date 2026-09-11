# EVENT_HS_UNI

![EVENT_HS_UNI](./EVENT_HS_UNI.svg)

* * * * * * * * * *

## Introduction

`EVENT_HS_UNI` is a unidirectional, dataless member of the `EVENT_HS` handshake family (IEC 61499 design pattern). It declares only a single **REQ** event that travels from the Plug side to the Socket side. In contrast to the full `EVENT_HS` adapter, it omits **CNF**, **IND**, and **RSP** entirely, reducing the interaction to a pure fire-and-forget notification.

This adapter is deliberately **not a real handshake**: the Socket side has no mechanism to acknowledge or refuse the request, and the Plug side has no way to determine whether the Socket actually received or acted upon it. It should only be used in scenarios where one-way, best-effort semantics are acceptable.

## Interface Structure

The adapter follows the standard Plug/Socket role split: the **Plug** side (`Name>>`) keeps the declared direction and fires **REQ**; the **Socket** side (`>>Name`) mirrors it and reacts to the incoming **REQ**.

### **Event Inputs**

None. The adapter declares no event inputs; all interaction is driven by the single output event.

### **Event Outputs**

| Name | Type | Comment |
|------|------|---------|
| REQ | Event | Request/notification from Plug to Socket, no reply expected |

### **Data Inputs**

None. The adapter is dataless; the **REQ** event carries no associated data.

### **Data Outputs**

None. No data is returned from the Socket side.

### **Adapters**

The adapter provides the standard two connector roles:

- **PLUG** (left interface): initiates the interaction by emitting **REQ**.
- **SOCKET** (right interface): receives and reacts to **REQ**.

The service sequence `notify` describes a single transaction: an input primitive on the PLUG interface with event **REQ** produces an output primitive on the SOCKET interface with the same event, forwarding the notification one way.

## Functionality

`EVENT_HS_UNI` implements a one-way, dataless notification channel. When the connected function block on the Plug side raises the **REQ** event, it is forwarded directly to the Socket side, where the corresponding function block reacts to it.

Because no acknowledgment path exists, the semantics are purely best-effort:

- The Plug side cannot know whether the Socket side received the event.
- The Socket side cannot confirm processing or reject the request.
- No retry, timeout, or handshake state tracking is possible at the adapter level.

The adapter is a reduction of the full `EVENT_HS` pattern, keeping only the request direction and dropping all reply mechanisms.

## Technical Features

- **Unidirectional event flow**: Only **REQ** is defined; no response events exist.
- **Dataless**: The event carries no payload, making it suitable for pure control signals or notifications.
- **Best-effort semantics**: No acknowledgment, no back-pressure, no delivery guarantee.
- **Standard Plug/Socket pattern**: Compatible with the IEC 61499 connector model and standard 4diac wiring practices.
- **Typed adapter family**: Part of the `adapter::types::bidirectional::Handshake` package, alongside other reduced variants.
- **Optional service documentation**: The embedded `<Service>` block is purely informational; it does not affect event forwarding at runtime.

## State Overview

The adapter has no internal state machine. It is a passive connector that forwards the **REQ** event from the Plug interface to the Socket interface. Any state handling must be implemented in the connected function blocks.

## Application Scenarios

- **Fire-and-forget notifications**: Signaling a peer component that an event occurred, where no confirmation is required.
- **One-directional control pulses**: Triggering a one-shot action on the Socket side (e.g., reset, start, stop command) where the sender does not need feedback.
- **Event broadcasting / logging**: Propagating an occurrence to a downstream component without expecting a reply.
- **Reduced-resource designs**: Where the full handshake overhead of `EVENT_HS` is unnecessary and every extra event line complicates the application.

Typical use cases include simple alarm signals, watch-dog pulses, or inter-module triggers where the communication channel is inherently reliable and loss is acceptable.

## Comparison with Similar Blocks

| Adapter | REQ | CNF | IND | RSP | Data | Semantics |
|---------|-----|-----|-----|-----|------|-----------|
| `EVENT_HS` | Yes | Yes | Yes | Yes | No | Full handshake, acknowledged request/response |
| `EVENT_HS_UNI` | Yes | No | No | No | No | Unidirectional fire-and-forget, no acknowledgment |
| `EVENT_HS_UNI_WSTRING` | Yes | No | No | No | Yes | Unidirectional with string payload, no acknowledgment |
| `EVENT_HS_ACK` | Yes | Yes | No | No | No | Request with acknowledgment, no independent indication |
| `EVENT_HS_ACK_WSTRING` | Yes | Yes | No | No | Yes | Request with acknowledgment and string payload |

Compared to the full `EVENT_HS`, `EVENT_HS_UNI` reduces the interface to a single event and drops all handshake semantics. It is therefore not a drop-in replacement where reliable delivery or confirmation is required.

## Conclusion

`EVENT_HS_UNI` provides a minimal, unidirectional, dataless communication channel in the `EVENT_HS` family. Its single **REQ** event enables fire-and-forget notifications between Plug and Socket with the least possible interface overhead. However, because it offers no acknowledgment or delivery guarantee, it must only be used where best-effort semantics are acceptable. For applications requiring reliable, confirmed handshakes, the full `EVENT_HS` or one of its acknowledged variants (`EVENT_HS_ACK`, `EVENT_HS_ACK_WSTRING`) is the appropriate choice.

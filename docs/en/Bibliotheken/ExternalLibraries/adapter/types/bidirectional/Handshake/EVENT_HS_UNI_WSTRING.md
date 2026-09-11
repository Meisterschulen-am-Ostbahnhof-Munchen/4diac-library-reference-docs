# EVENT_HS_UNI_WSTRING

![EVENT_HS_UNI_WSTRING](./EVENT_HS_UNI_WSTRING.svg)

* * * * * * * * * *

## Introduction

The **EVENT_HS_UNI_WSTRING** adapter is a data-carrying, reduced member of the `EVENT_HS` (Handshake) adapter family, implementing a unidirectional, fire-and-forget communication pattern. It declares a single **REQ** event paired with a **REQD** WSTRING payload that flows exclusively from the Plug to the Socket side. Unlike a true handshake, this adapter provides no acknowledgment, no indication, and no response channel whatsoever — the Socket side cannot confirm receipt or reject the request, and the Plug side has no way of verifying that the message was ever delivered or processed.

This adapter is designed for genuine one-way, best-effort notifications where the simplicity of a single event+payload pair outweighs the need for end-to-end reliability. It follows the "name,value" message style (e.g., `push,100`) commonly used in lightweight messaging systems.

## Interface Structure

The adapter exposes a minimal interface with only one event and one data output on the Plug side, mirrored to the Socket side. There are no inputs on either role.

### **Event Inputs**

None. The adapter declares no event inputs on either the Plug or Socket interface.

### **Event Outputs**

| Name | Type | With | Comment |
|------|------|------|---------|
| **REQ** | Event | REQD | Request/notification from Plug to Socket, no reply expected |

The **REQ** event is the sole trigger of the adapter. When fired on the Plug side, it is forwarded to the Socket side, carrying the current value of **REQD** as its payload.

### **Data Inputs**

None. The adapter declares no data inputs on either role.

### **Data Outputs**

| Name | Type | Comment |
|------|------|---------|
| **REQD** | WSTRING | Request payload, accompanies REQ |

The **REQD** output holds the payload that is transmitted together with the **REQ** event. It is a wide string (WSTRING) that can carry arbitrary textual content, typically structured in a `name,value` format. The value is sampled at the moment the **REQ** event fires and is delivered to the Socket side.

### **Adapters**

The adapter defines the standard Plug/Socket role split:

- **PLUG** (interface name: `Name>>`): The initiating side. It owns the declared direction and fires **REQ** carrying **REQD**.
- **SOCKET** (interface name: `>>Name`): The receiving side. It mirrors the **REQ** event and receives the **REQD** payload for reaction.

The service sequence `notify` documents the single transaction: a Plug-side `REQ` input primitive with parameter `REQD` is propagated as a Socket-side `REQ` output primitive with the same parameter.

## Functionality

The adapter implements a single, atomic, unidirectional transaction:

1. The application connected to the **Plug** side sets the **REQD** data output to the desired payload value.
2. The **REQ** event is fired on the Plug side.
3. The adapter forwards the **REQ** event to the **Socket** side, passing along the current value of **REQD**.
4. The application connected to the **Socket** side receives the **REQ** event and reads the **REQD** payload.

There is no second transaction step. No `CNF` (confirmation), `IND` (indication), or `RSP` (response) events exist — neither with nor without payload. The communication is strictly one-way and fire-and-forget.

The `<Service>` block included in the adapter definition is purely documentation. Per the XSD's `minOccurs="0"`, a service sequence diagram is optional and not required for the adapter connection to forward events at runtime. It is included to make the single REQ transaction explicit and to aid designers reading the adapter documentation.

## Technical Features

- **Single event/payload pair**: Exactly one event (`REQ`) and one data output (`REQD`) constitute the entire interface, minimizing connection overhead.
- **WSTRING payload**: The payload type is `WSTRING`, supporting Unicode text and arbitrary-length string data, suitable for name/value pairs, JSON snippets, or command strings.
- **Unidirectional flow**: Data and events travel exclusively from Plug to Socket. There is no reverse channel.
- **No handshake semantics**: Deliberately omits confirmation, indication, and response events. The Socket side cannot acknowledge or refuse, and the Plug side cannot detect delivery failure.
- **Zero state complexity**: With only a single transaction and no reply path, the adapter requires no internal state machine beyond event forwarding.
- **Standard Plug/Socket roles**: Consistent with the wider `EVENT_HS` family, preserving the conventional interface naming and direction conventions of IEC 61499 adapter types.

## State Overview

The adapter does not maintain any internal state beyond the propagation of a single event. Its behavior can be modeled with a trivial two-state view:

- **Idle**: No transaction in progress. The adapter is ready to accept a `REQ` on the Plug side.
- **Transmitting**: The `REQ` event with the `REQD` payload is being forwarded from Plug to Socket. This state is instantaneous — the forwarding is atomic and completes within the same execution cycle.

There is no waiting, retry, or timeout state because the adapter does not expect a reply. Once the event is forwarded, the adapter returns to the Idle state. The absence of `CNF` or `RSP` events means there is no suspension of the execution resource waiting for a counterpart response.

## Application Scenarios

The **EVENT_HS_UNI_WSTRING** adapter is suitable for scenarios where one-way notification with a small payload is acceptable and no acknowledgment is required:

- **Telemetry and status push**: Sensors or devices pushing measurement values (e.g., `temperature,23.5`) to a monitoring unit that does not need to confirm receipt.
- **Logging and audit trails**: Sending log entries or audit messages to a central collector where occasional loss is tolerable.
- **Command dispatch (fire-and-forget)**: Sending non-critical commands (e.g., `push,100`) where the receiving side is expected to act on them without the sender waiting for confirmation.
- **Event broadcasting**: Propagating system events (e.g., `mode,automatic`) to interested consumers in a one-to-many or one-to-one topology where reliability is not critical.
- **Integration with message-style protocols**: Matching the "name,value" textual message pattern documented in the Handshake design pattern, useful for bridging IEC 61499 applications to lightweight messaging middleware.

It should **not** be used when the sender needs to know whether the request was received or processed, when a reply is required, or when guaranteed delivery is a system requirement.

## Comparison with Similar Blocks

The **EVENT_HS** family contains multiple variants tailored to different handshake depths. The following comparison highlights the key differences:

| Adapter | REQ/REQD | CNF/CNFD | IND/INDD | RSP/RSPD | Payload | Use Case |
|---------|----------|----------|----------|----------|---------|----------|
| **EVENT_HS_UNI_WSTRING** | Yes | No | No | No | WSTRING | One-way fire-and-forget with payload |
| **EVENT_HS_UNI** | Yes | No | No | No | None | One-way fire-and-forget, no payload |
| **EVENT_HS_ACK** | Yes | Yes | No | No | None | One-way with acknowledgement |
| **EVENT_HS_ACK_WSTRING** | Yes | Yes | No | No | WSTRING | One-way with acknowledgement and payload |
| **EVENT_HS** (full) | Yes | Yes | Yes | Yes | Mixed | Complete bidirectional handshake |

Compared to the full `EVENT_HS`, this adapter sacrifices all reply capability for interface simplicity. Compared to `EVENT_HS_UNI`, it adds a WSTRING payload at the cost of slightly more connection wiring. Compared to the `_ACK` variants, it omits confirmation, trading reliability for minimalism.

## Conclusion

The **EVENT_HS_UNI_WSTRING** adapter provides a lean, purpose-built solution for unidirectional, payload-carrying notifications within the IEC 61499 adapter framework. Its single `REQ`/`REQD` event-payload pair offers a clear and maintainable interface for fire-and-forget communication, while its deliberate exclusion of handshake mechanisms keeps the design simple and avoids the complexity of error handling and timeouts.

As a reduced member of the `EVENT_HS` family, it is best employed where best-effort semantics are acceptable and where the "name,value" message style aligns with the surrounding system design. For scenarios demanding acknowledgment or bidirectional interaction, the companion adapters (`EVENT_HS_ACK_WSTRING`, full `EVENT_HS`) provide richer handshake capabilities.
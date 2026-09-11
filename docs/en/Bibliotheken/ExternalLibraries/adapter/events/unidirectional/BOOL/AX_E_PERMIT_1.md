# AX_E_PERMIT_1

![AX_E_PERMIT_1](./AX_E_PERMIT_1.svg)

* * * * * * * * * *

## Introduction

AX_E_PERMIT_1 is a compact event gating function block that propagates a single event channel only when a permit condition is satisfied. Instead of using a direct Boolean data input, the permit condition is provided through a unidirectional adapter socket named `PERMIT`. This makes the block suitable for modular automation solutions in which permission logic is separated from the event consumer.

The block follows the IEC 61499-1 model and is declared as a generic function block type. It has one event input, one event output, and no dedicated data inputs or data outputs. All condition information is received through the adapter interface.

## Interface Structure

### **Event Inputs**

| Name | Type | Comment |
|------|------|---------|
| `EI1` | Event | Event input channel 1 |

### **Event Outputs**

| Name | Type | Comment |
|------|------|---------|
| `EO1` | Event | Event output channel 1 |

### **Data Inputs**

There are no data inputs.

### **Data Outputs**

There are no data outputs.

### **Adapters**

| Direction | Name | Type | Comment |
|-----------|------|------|---------|
| Socket | `PERMIT` | `adapter::types::unidirectional::AX` | Permit condition adapter input |

The block contains a single adapter socket. The connected adapter supplies the permit or enable condition used to decide whether an incoming event is forwarded.

## Functionality

AX_E_PERMIT_1 acts as a permissive event gate.

When an event arrives at `EI1`, the block evaluates the condition received through the `PERMIT` adapter socket. If the condition is active, the event is immediately propagated to `EO1`. If the condition is not active, the event is suppressed and no output event is generated.

The adapter is unidirectional, meaning that the condition data flows from the connected adapter into the socket. The block itself does not send data or events back through the adapter. This keeps the permission source decoupled from the event handling logic.

Because the block has no data inputs, the adapter connection replaces the conventional `PERMIT` Boolean input that would otherwise be needed. This allows a permission signal to come from a reusable adapter-based logic component without adding explicit data wires to the function block network.

## Technical Features

- Single-channel event permit gating.
- Adapter-based condition input using `adapter::types::unidirectional::AX`.
- No direct data inputs or data outputs.
- Suitable for generic use through the declared generic class name `GEN_AX_E_PERMIT`.
- Compact interface with only one event input, one event output, and one adapter socket.
- Compatible with IEC 61499-1 Annex A type identification.
- The logic structure makes the block easy to reuse in different permission and interlock scenarios.

## State Overview

The function block type does not define an explicit state machine. From the IEC 61499 point of view, the block behaves as a stateless event gate.

Conceptually, the following behavior can be expected:

| Condition | Behavior |
|-----------|----------|
| Event arrives at `EI1` and permit is active | `EO1` is triggered |
| Event arrives at `EI1` and permit is inactive | The event is ignored |
| No event arrives at `EI1` | No output event is generated |

Since no internal state is stored between events, the block returns to its idle condition after each event processing step.

## Application Scenarios

AX_E_PERMIT_1 is useful wherever an event must be allowed or blocked by an external condition. Typical scenarios include:

- Machine start commands that are only allowed when a safety permission is present.
- Event propagation controlled by an operating mode selector.
- Interlock logic where a process event must be suppressed until a precondition is fulfilled.
- Modular automation architectures where permission signals are provided by reusable adapter blocks.
- Communication between independent control modules that share a common enable condition through an adapter network.

The adapter-based design is particularly helpful when the same permission source must be connected to multiple event consumers without creating complex data wiring.

## Comparison with Similar Blocks

| Block or Approach | Characteristics | Difference |
|-------------------|-----------------|-------------|
| `E_PERMIT` with Boolean input | Uses a direct `PERMIT` data input | Simpler wiring, but permission source must be connected through an explicit Boolean data connection |
| `E_SWITCH` | Routes an event to one of several outputs based on a selector input | Performs selection, not simple pass/block behavior |
| `E_SPLIT` | Forwards one event to multiple outputs | Does not apply a permission condition |
| Generic adapter-based gate | Uses an adapter socket for the enable condition | More modular and reusable, but requires a compatible adapter connection |

Compared with a direct Boolean implementation, AX_E_PERMIT_1 trades a small amount of wiring simplicity for a much cleaner and more modular interface. The adapter connection allows the permission logic to be encapsulated, reused, and independently maintained.

## Conclusion

AX_E_PERMIT_1 is a focused and reusable event gating block. Its adapter-based permit interface makes it well suited for modular automation systems where conditions are managed through adapter connections rather than plain data signals. With one event input, one event output, and one adapter socket, it provides a simple way to implement permissive event propagation while keeping the overall function block structure clear and maintainable.

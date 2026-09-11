# AX_E_PERMIT_INVERT_3


![AX_E_PERMIT_INVERT_3_network](./AX_E_PERMIT_INVERT_3_network.svg)

![AX_E_PERMIT_INVERT_3](./AX_E_PERMIT_INVERT_3.svg)

* * * * * * * * * *

## Introduction

AX_E_PERMIT_INVERT_3 is a composite subapplication for 3-channel event gating with an inverted adapter-based enable signal. It combines an adapter inverter AX_NOT_INIT and a 3-channel event permit gate AX_E_PERMIT_3. The subapplication forwards events only when the external PERMIT signal is FALSE; when PERMIT is TRUE, all event channels are blocked.

## Interface Structure

### **Event Inputs**

| Name | Type | Description |
| --- | --- | --- |
| EI1 | Event | Event input channel 1 |
| EI2 | Event | Event input channel 2 |
| EI3 | Event | Event input channel 3 |

### **Event Outputs**

| Name | Type | Description |
| --- | --- | --- |
| EO1 | Event | Event output channel 1 |
| EO2 | Event | Event output channel 2 |
| EO3 | Event | Event output channel 3 |

### **Data Inputs**

This subapplication has no explicit data inputs.

### **Data Outputs**

This subapplication has no explicit data outputs.

### **Adapters**

| Name | Type | Direction | Description |
| --- | --- | --- | --- |
| PERMIT | adapter::types::unidirectional::AX | Socket | Active-low permit signal. The boolean value carried by this adapter is inverted internally; events are passed only when this value is FALSE. |

## Functionality

The function is achieved by two internal components:

1. `AX_NOT_INIT` acts as a logical NOT operator on the adapter-based boolean signal connected to `PERMIT`.
2. `AX_E_PERMIT_3` is a three-channel event gate. It receives events from `EI1` ... `EI3` and forwards them to `EO1` ... `EO3` only when its own `PERMIT` input is active.

The external `PERMIT` adapter is connected to `AX_NOT_INIT.IN`. The inverted result is sent from `AX_NOT_INIT.OUT` to `AX_E_PERMIT_3.PERMIT`. Thus, the effective gate condition is the opposite of the external signal:

| External PERMIT | Signal seen by gate | Event behavior |
| --- | --- | --- |
| FALSE | TRUE | Events are forwarded |
| TRUE | FALSE | Events are blocked |

Each event channel is independent: `EI1` is routed through the gate to `EO1`, `EI2` to `EO2`, and `EI3` to `EO3`. The same inverted permit value applies to all three channels.

## Technical Features

- Three independent event input/output channels.
- Inverted enable behavior using an adapter-based boolean inverter.
- No explicit data inputs or outputs; the permission state is transported through the `PERMIT` adapter.
- One unidirectional adapter socket of type `adapter::types::unidirectional::AX`.
- Internal composition consists of `AX_NOT_INIT` and `AX_E_PERMIT_3`.
- The subapplication has no ECC or state machine; it is a purely event-routing composition.

## State Overview

This subapplication does not define internal persisting states. The behavior is determined solely by the current boolean value carried by the `PERMIT` adapter. From an external perspective, the gate is either open or closed:

- **Open**: External `PERMIT` is `FALSE`, the gate receives an active permit, and arriving events are released on the matching event output.
- **Closed**: External `PERMIT` is `TRUE`, the gate receives an inactive permit, and arriving events are suppressed.

Because there is no internal state memory, the behavior is deterministic and independent of previous event occurrences.

## Application Scenarios

Typical use cases include:

- **Active-low enable signals**: An application that should run while an enable signal is de-asserted.
- **Safety/inhibit gating**: A machine control event may be allowed only while an inhibit flag is `FALSE`.
- **Multi-channel event distribution**: Three related event paths, such as preparation, execution, and completion, can be opened or closed together using one common inverted permission signal.
- **Adapter-based software architectures**: Used in IEC 61499 systems where boolean status information is exchanged via unidirectional adapters rather than separate data connections.

## Comparison with Similar Blocks

`AX_E_PERMIT_3` is the direct non-inverted counterpart. It forwards events when its `PERMIT` adapter value is `TRUE`. `AX_E_PERMIT_INVERT_3` inserts `AX_NOT_INIT` before the same gate, so the permission condition becomes inverted. In systems where the available inhibit signal is active-high, this avoids manually inverting the signal outside the subapplication.

| Block | Permits events when external PERMIT is |
| --- | --- |
| `AX_E_PERMIT_3` | `TRUE` |
| `AX_E_PERMIT_INVERT_3` | `FALSE` |

This makes the block suitable for applications that need an event gate with active-low enable logic while retaining the same channel structure as the standard `AX_E_PERMIT_3`.

## Conclusion

`AX_E_PERMIT_INVERT_3` is a compact, reusable subapplication for event gating with inverted adapter-based permission. By combining an adapter inverter with a 3-channel event gate, it provides a clean solution for applications that must pass events only while the permit signal is `FALSE`. Its simple internal structure and absence of data pins make it easy to integrate in event-driven IEC 61499 systems.
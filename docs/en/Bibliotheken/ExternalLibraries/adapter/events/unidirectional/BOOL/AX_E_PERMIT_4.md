# AX_E_PERMIT_4

![AX_E_PERMIT_4](./AX_E_PERMIT_4.svg)

* * * * * * * * * *

## Introduction

AX_E_PERMIT_4 is a generic function block designed for conditional propagation of four independent event channels. It acts as a permissive gate: each incoming event at one of its four event inputs is forwarded to the corresponding event output only if the attached permit condition via the unidirectional adapter socket `PERMIT` allows it. This block is particularly useful in safety-oriented or mode-dependent control applications where event flow must be gated by an external authorization signal.

## Interface Structure

### **Event Inputs**

| Event | Type   | Description               |
|-------|--------|---------------------------|
| EI1   | Event  | Event input channel 1     |
| EI2   | Event  | Event input channel 2     |
| EI3   | Event  | Event input channel 3     |
| EI4   | Event  | Event input channel 4     |

### **Event Outputs**

| Event | Type   | Description                |
|-------|--------|----------------------------|
| EO1   | Event  | Event output channel 1     |
| EO2   | Event  | Event output channel 2     |
| EO3   | Event  | Event output channel 3     |
| EO4   | Event  | Event output channel 4     |

### **Data Inputs**

This function block has no data inputs.

### **Data Outputs**

This function block has no data outputs.

### **Adapters**

| Socket   | Type                                               | Description                       |
|----------|----------------------------------------------------|-----------------------------------|
| PERMIT   | adapter::types::unidirectional::AX                | Unidirectional permit condition input |

## Functionality

AX_E_PERMIT_4 implements a permissive event propagation scheme. Each pair of input/output events (EI1/EO1, EI2/EO2, EI3/EO3, EI4/EO4) forms an independent channel. When an event is received on one of the event inputs, the block checks the current value of the `PERMIT` adapter. If the permit condition evaluates to true (permission granted), the corresponding event is released on the associated output. If the permit condition is false, the event is suppressed and no output is generated.

The block is generic, meaning it can be instantiated in a project with a concrete adapter type that matches the `adapter::types::unidirectional::AX` interface. The unidirectional nature of the adapter means that the permit signal flows only from the connected partner into this block, without requiring any return data.

## Technical Features

- **Generic instantiation**: Declared with `eclipse4diac::core::GenericClassName` equal to `'GEN_AX_E_PERMIT'`, enabling type replacement at instantiation time.
- **Four independent channels**: Allows simultaneous handling of up to four separate event streams with identical gating behavior.
- **Unidirectional adapter**: The permit signal is supplied through a unidirectional adapter socket, simplifying the connection to a permit source (e.g., a safety interlock or operator authorization block).
- **No data processing**: The block does not modify or buffer any data; it purely gates event occurrence.
- **Package compliance**: Aligned with the IEC 61499-1 Annex A standard and located in the `adapter::events::unidirectional` compiler package.

## State Overview

The function block does not maintain an explicit internal state machine. Its behavior is combinatorial with respect to events:

- **Permit Granted**: An input event immediately triggers the corresponding output event.
- **Permit Denied**: An input event is discarded; no output event is emitted.

The only dynamic condition is derived from the external permit signal carried by the adapter. Consequently, the block is stateless between event occurrences and responds identically to each event based on the current permit value.

## Application Scenarios

- **Safety interlocks**: Suppressing start commands or motion signals when a safety condition is not satisfied.
- **Mode-based operation**: Allowing or blocking commands depending on the current operational mode (e.g., manual vs. automatic).
- **Conditional alarming**: Forwarding alarm events only when a monitoring system is armed.
- **Line gating in production cells**: Passing control events to downstream equipment only when the upstream permit is active.

## Comparison with Similar Blocks

| Feature                      | AX_E_PERMIT_4                          | AX_E_PERMIT (single channel)          | Standard event propagation (no permit) |
|------------------------------|----------------------------------------|---------------------------------------|----------------------------------------|
| Number of event channels     | 4                                      | 1                                     | 1 or more (direct wiring)              |
| Permit gating               | Yes (via unidirectional adapter)      | Yes (via unidirectional adapter)     | No                                     |
| Generic type support        | Yes                                    | Yes                                   | Not applicable                         |
| Data interface              | None                                   | None                                  | Usually none                           |
| Typical use                 | Multi-channel safety or mode gating    | Simple single-channel gate            | Unconditional event forwarding         |

The main advantage of AX_E_PERMIT_4 over a single-channel variant is its compactness when four independent event streams must be gated by the same permit condition. Compared to direct event wiring, it adds a controlled suppression layer, which is essential in safety-related architectures.

## Conclusion

AX_E_PERMIT_4 provides a straightforward and reusable solution for conditional event propagation across four independent channels. Its generic design and dependence on a unidirectional permit adapter make it a flexible building block in IEC 61499-based control applications that require event gating under external authorization. The block is easy to integrate, requires no data handling, and contributes to cleaner and more maintainable safety and mode-management logic.

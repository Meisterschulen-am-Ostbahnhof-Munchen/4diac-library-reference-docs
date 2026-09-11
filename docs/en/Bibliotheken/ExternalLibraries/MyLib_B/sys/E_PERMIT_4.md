# E_PERMIT_4


![E_PERMIT_4_network](./E_PERMIT_4_network.svg)

![E_PERMIT_4](./E_PERMIT_4.svg)

* * * * * * * * * *
## Introduction

The **E_PERMIT_4** is a composite subapplication that bundles four independent event-permission gates (based on the standard IEC 61499 `E_PERMIT` function block) into a single reusable component. It provides four event channels, each of which can selectively pass or block an incoming event depending on a common Boolean permit condition. This design simplifies the integration of multi-channel event gating logic in distributed control applications.

## Interface Structure

### **Event Inputs**

| Name | Type   | Description                    |
|------|--------|--------------------------------|
| EI1  | Event  | Event input channel 1          |
| EI2  | Event  | Event input channel 2          |
| EI3  | Event  | Event input channel 3          |
| EI4  | Event  | Event input channel 4          |

### **Event Outputs**

| Name | Type   | Description                     |
|------|--------|---------------------------------|
| EO1  | Event  | Event output channel 1          |
| EO2  | Event  | Event output channel 2          |
| EO3  | Event  | Event output channel 3          |
| EO4  | Event  | Event output channel 4          |

### **Data Inputs**

| Name   | Type  | Description                                |
|--------|-------|--------------------------------------------|
| PERMIT | BOOL  | Permit condition shared by all four channels |

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

The subapplication contains four internal instances of the standard `E_PERMIT` function block, connected as follows:

- `EI1` → `E_PERMIT_1.EI` → `EO1`
- `EI2` → `E_PERMIT_2.EI` → `EO2`
- `EI3` → `E_PERMIT_3.EI` → `EO3`
- `EI4` → `E_PERMIT_4.EI` → `EO4`

The single Boolean input `PERMIT` is distributed to all four internal blocks.

The operating behavior for each channel is identical:

1. An event arrives at the corresponding event input (e.g., `EI1`).
2. The internal `E_PERMIT` block evaluates the shared `PERMIT` signal.
3. If `PERMIT` is **TRUE**, the event is immediately forwarded to the associated event output (e.g., `EO1`).
4. If `PERMIT` is **FALSE**, the event is discarded and no output event is generated.

The channels are fully independent in terms of event flow, meaning an event on one channel does not influence the others, apart from sharing the same permit value.

## Technical Features

- **Four-channel event gating** in a single compact subapplication.
- **One shared permit input** (`PERMIT`) controlling all channels simultaneously, reducing wiring complexity in higher-level applications.
- **Based on standard IEC 61499** `E_PERMIT` blocks, ensuring predictable, platform-independent behavior.
- **No internal state retention** – the gate is purely event-driven; each event is processed on arrival.
- **Scalable concept** – the pattern can be extended to more channels if required.
- **Event propagation delay** is minimal, as each channel is a direct pass-through path when permitted.

## State Overview

The `E_PERMIT_4` subapplication does not maintain any persistent internal state. As it is composed of `E_PERMIT` blocks, each individual gate behaves in a stateless combinational manner with respect to events:

- When no event is present, the subapplication is idle.
- Upon an event input, the gate either forwards (permit = TRUE) or drops (permit = FALSE) the event.
- Changes to `PERMIT` take effect immediately for subsequent events.

There are no hidden latches, timers, or counters inside the component.

## Application Scenarios

- **Multi-channel access control**: Enable or disable several independent event streams (e.g., sensor triggers) based on a single enabling condition.
- **Safe stop / emergency gating**: If a safety signal (`PERMIT`) becomes FALSE, all outgoing event triggers are suppressed.
- **Sequencing gates**: Conditionally forward start or stop commands to multiple machine axes simultaneously.
- **Test & simulation**: Provide a global switch to block all event-driven communication while simulating or debugging.
- **Batch processing**: Allow a set of parallel process steps only when a common prerequisite is fulfilled.

## Comparison with Similar Blocks

| Feature                      | E_PERMIT_4                         | Single E_PERMIT                     | E_SWITCH (per MB)                    |
|------------------------------|------------------------------------|-------------------------------------|--------------------------------------|
| Number of channels           | 4                                  | 1                                   | 1 (with 2 outputs)                   |
| Permit input                 | Single shared `PERMIT`             | Dedicated `PERMIT` per instance     | Not applicable                       |
| Event routing                | Pass or block                      | Pass or block                       | Route to one of two outputs          |
| Use case                     | Multi-channel gating               | Single-channel gating               | Branching / multiplexing events      |
| Integration effort           | Low (one component)                | High for many channels              | Medium for branching logic           |

Compared to placing four separate `E_PERMIT` blocks manually, `E_PERMIT_4` offers a cleaner interface, consistent wiring, and a single control point for the permit condition. It does not provide the branching capability of an `E_SWITCH`, but it is purpose-built for independent channel blocking.

## Conclusion

The `E_PERMIT_4` subapplication is a practical and reusable building block for applications that require simultaneous enable/disable control over multiple event streams. By wrapping four standard `E_PERMIT` blocks behind a unified interface, it reduces engineering effort, improves readability of the application logic, and ensures consistent behavior across all channels. It is particularly well suited for access control, safety gating, and multi-axis coordination tasks within IEC 61499-based systems.
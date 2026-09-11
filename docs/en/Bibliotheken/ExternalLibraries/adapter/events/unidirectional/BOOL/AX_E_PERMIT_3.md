# AX_E_PERMIT_3

![AX_E_PERMIT_3](./AX_E_PERMIT_3.svg)

* * * * * * * * * *

## Introduction

The **AX_E_PERMIT_3** is a generic function block (FB) designed for permissive propagation of three independent event channels. It acts as a gate that forwards each incoming event to its corresponding output only when a global enable condition is satisfied. The enable condition is provided via an adapter interface, making the block highly reusable in various control scenarios where event flow must be conditionally allowed.

This FB is part of the unidirectional event adapter family, specifically tailored for applications that require a single permission signal to control multiple event streams. It is defined as a generic FB, meaning its behavior can be instantiated and customized through type attributes.

## Interface Structure

The interface consists of three event input channels, three matching event output channels, and one adapter socket for the permission signal. There are no data inputs or outputs.

### **Event Inputs**

| Name | Data Type | Description |
|------|-----------|-------------|
| EI1  | Event      | Event input channel 1 – triggers propagation to EO1 if permitted. |
| EI2  | Event      | Event input channel 2 – triggers propagation to EO2 if permitted. |
| EI3  | Event      | Event input channel 3 – triggers propagation to EO3 if permitted. |

### **Event Outputs**

| Name | Data Type | Description |
|------|-----------|-------------|
| EO1  | Event      | Event output channel 1 – emits an event when EI1 occurs and permission is granted. |
| EO2  | Event      | Event output channel 2 – emits an event when EI2 occurs and permission is granted. |
| EO3  | Event      | Event output channel 3 – emits an event when EI3 occurs and permission is granted. |

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

| Type | Role | Name | Description |
|------|------|------|-------------|
| adapter::types::unidirectional::AX | Socket | PERMIT | Provides a unidirectional permission signal. The adapter must be connected to a compatible plug that supplies an enable/disable condition. The exact semantics (e.g., boolean value) depend on the specific adapter implementation. |

## Functionality

The FB implements a simple gating mechanism for three independent event streams. When an event arrives on any of the input channels (EI1, EI2, or EI3), the FB checks the current state of the **PERMIT** adapter. If the permission condition is **true** (or "allowed"), the corresponding output event (EO1, EO2, or EO3 respectively) is emitted. If the permission condition is **false**, the event is discarded and no output event occurs.

The propagation is **not mutually exclusive**: each channel operates independently, and the permission check is identical for all three channels at any given moment. This means that a single permission signal can simultaneously enable or disable all three event paths.

Since the FB has no data inputs or outputs, the permission condition is entirely derived from the adapter. The adapter is expected to provide a boolean-like signal, but the exact representation is defined by the underlying adapter type (in this case, `unidirectional::AX`).

The block is **stateless** in the sense that it does not maintain any internal memory; the decision to forward an event is made at the moment the event occurs based solely on the current adapter value.

## Technical Features

- **Generic Implementation**: The FB is marked with the attribute `eclipse4diac::core::GenericClassName = 'GEN_AX_E_PERMIT'`, indicating that it serves as a generic template. This allows the system to instantiate a specialized version of the block for specific adapter definitions via type generation mechanisms.
- **Three-Channel Design**: Supports exactly three event input/output pairs, making it suitable for applications with three parallel event streams.
- **Unidirectional Adapter**: Uses a socket adapter of type `unidirectional::AX`, which is a unidirectional connection. This ensures a clear data flow direction for the permission signal.
- **No Data Interface**: Simplifies usage by eliminating data handling; only event flow control is performed.
- **Direct Event Mapping**: Each input event is mapped one-to-one to its output event with identical naming conventions (EI1→EO1, EI2→EO2, EI3→EO3), simplifying wiring and readability.

## State Overview

The FB does not implement an explicit state machine. It operates **combinatorially** with respect to events: each event is processed independently and immediately. There is no history or internal state that could affect subsequent events. The block can be considered as a pure function: given an event on an input and a current permission value, it either emits an output event or does nothing.

In practice, the only "state" is the value of the adapter signal, which is external to the FB.

## Application Scenarios

- **Safety Interlocks**: Use a single safety relay or enable signal to block or allow multiple control commands (e.g., start/stop/emergency) simultaneously.
- **Mode Selection**: In machinery, a mode selector can enable or disable a set of event-driven actions (e.g., automatic, manual, maintenance modes).
- **Conditional Data Flow**: When three independent event sources must only be forwarded when a master condition holds (e.g., a supervisory system grants permission).
- **Event Gating in Distributed Systems**: In IEC 61499 based applications, this FB can be used to gate events between function blocks without complex logic, relying on an adapter to provide the permission.
- **Test and Simulation**: The block can be used to simulate an enable/disable switch for a group of event paths in a model.

## Comparison with Similar Blocks

Given the generic nature of the FB, several similar blocks exist, often differing in the number of channels or the type of permission signal:

- **AX_E_PERMIT_1** / **AX_E_PERMIT_2** (if they exist): Single- or two-channel versions, where the same permission is applied to fewer event streams. The three-channel version provides a balance between simplicity and parallelism.
- **AX_E_GATE** (generic): May implement a more complex gating logic, possibly with per-channel enable signals instead of a single global permission.
- **AX_E_SELECT** (if available): Might route events to different outputs based on a condition, rather than simply allowing or blocking them.
- **Adapter-Based vs. Data-Based Gating**: Compared to a block with a boolean data input (e.g., `E_PERMIT`), using an adapter decouples the permission source from the FB itself, enabling reuse in different contexts without changing the FB’s interface. This makes the adapter-based approach more flexible and modular.

The key differentiator of **AX_E_PERMIT_3** is its **explicit adapter interface** and its **fixed number of three independent channels**, which suits mid-complexity event management tasks.

## Conclusion

The **AX_E_PERMIT_3** function block provides a clean, deterministic solution for conditionally propagating three independent event streams under a single permission signal. Its design, leveraging a unidirectional adapter, promotes modularity and reusability in IEC 61499 applications. The simple event-to-event mapping and lack of data handling make it easy to integrate into existing control logic, while its generic nature allows for adaptation to specific adapter definitions when needed.

By offering a straightforward gating mechanism, this block contributes to safer and more controlled event-driven execution in automated systems.

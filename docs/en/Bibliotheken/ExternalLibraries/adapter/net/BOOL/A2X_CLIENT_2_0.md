# A2X_CLIENT_2_0

![A2X_CLIENT_2_0](./A2X_CLIENT_2_0.svg)

* * * * * * * * * *

## Introduction

A2X_CLIENT_2_0 is a composite function block that takes two Boolean values (UP and DOWN) from an A2X adapter socket and performs a remote OPC-UA write via an embedded CLIENT_2_0 client. Each incoming value change is captured by its own edge-triggered D flip-flop (E_D_FF) before the write request is issued. This buffering ensures that only the most recent state changes are sent to the remote server, while the rest of the network remains decoupled from the exact timing of the write operation.

## Interface Structure

### Event Inputs

| Name | Type | Comment | With Variables |
|------|------|---------|----------------|
| INIT | EInit | Initialization | QI, ID |

### Event Outputs

| Name | Type | Comment | With Variables |
|------|------|---------|----------------|
| INITO | EInit | Initialization Confirm | QO, STATUS |
| CNF | Event | Data was sent | QO, STATUS |

### Data Inputs

| Name | Type | Comment |
|------|------|---------|
| QI | BOOL | Initialization qualifier, passed directly to the embedded CLIENT_2_0 |
| ID | WSTRING | Connection identifier / endpoint address for the OPC-UA server |

### Data Outputs

| Name | Type | Comment |
|------|------|---------|
| QO | BOOL | Quality / state output of the embedded CLIENT_2_0 |
| STATUS | WSTRING | Status message, holding the last operation result |

### Adapters

| Name | Type | Direction | Comment |
|------|------|-----------|---------|
| IN | adapter::types::unidirectional::A2X | Socket | Provides UP/DOWN Boolean values and their change events (E_UP / E_DOWN) |

## Functionality

The block is initialized via the INIT event. On INIT, the QI and ID input values are forwarded to the internal CLIENT_2_0, and the client establishes the connection to the configured OPC-UA server. The INITO event confirms successful completion and reports the resulting QO and STATUS.

At runtime, the adapter socket IN delivers the two Boolean values UP and DOWN. Each value is connected to the data input D of its own edge-triggered D flip-flop:
- IN.UP → E_D_FF_UP.D
- IN.DOWN → E_D_FF_DOWN.D

The corresponding adapter events (IN.E_UP and IN.E_DOWN) act as clock signals (CLK) for the respective flip-flops. When either event occurs, the current value on its data input is latched into the flip-flop output (Q). At the same time, the flip-flop's event output (EO) triggers a REQ event on the embedded CLIENT_2_0.

The latched values are connected to the client's data inputs:
- E_D_FF_UP.Q → CLIENT_2_0.SD_1
- E_D_FF_DOWN.Q → CLIENT_2_0.SD_2

All other values (the unchanged one) remain at their previous latched state. The REQ event therefore causes a single OPC-UA write operation, transmitting both SD_1 and SD_2 to the remote server. Upon completion, the client outputs the CNF event together with the current QO and STATUS values.

## Technical Features

- **Edge-triggered buffering**: Two independent E_D_FF instances capture UP and DOWN changes, guaranteeing that only actual state transitions cause a write operation.
- **Shared write channel**: Both flip-flop outputs lead to a single CLIENT_2_0 REQ input, ensuring that one event produces exactly one write request.
- **Direct initialization pass-through**: QI and ID are forwarded unchanged to the CLIENT_2_0, keeping the configuration interface minimal.
- **Combined result reporting**: Both INITO and CNF events deliver the same QO and STATUS data, giving a consistent view of the connection and operation state.

## State Overview

The block behaves as follows:

- **Uninitialized**: Before INIT, the internal CLIENT_2_0 is inactive. No events are processed and no writes occur.
- **Initializing**: After INIT arrives, the client is configured with QI and ID. Successful completion is signalled by INITO; the client is then ready for REQ.
- **Idle / Waiting**: Between REQ events, the flip-flops wait for new adapter events (E_UP or E_DOWN). Their outputs hold the last written values.
- **Write in progress**: When E_UP or E_DOWN fires, the corresponding flip-flop updates its output and triggers REQ. The write operation is performed by the client; its completion is signalled by CNF.

## Application Scenarios

- **Remote up/down control**: Sending two binary command states (e.g., raise/lower) from a local adapter to an OPC-UA server, where each state change must be individually acknowledged.
- **Binary state mirroring**: Continuously synchronizing two Boolean process variables to a remote controller, while avoiding continuous polling or redundant writes.
- **Distributed automation**: In a system where the A2X adapter provides field-level signals, this block forwards them to a central OPC-UA data hub, independent of the client's polling cycles.

## Comparison with Similar Blocks

- **Without buffering (direct connection)**: A straightforward CLIENT_2_0 with direct adapter connections would only write when a client-internal trigger occurs; it would not automatically react to every value change. A2X_CLIENT_2_0 ensures a write is issued for every transition.
- **Single E_D_FF variant**: A version using only one flip-flop would concatenate both values into a single data point, losing independence of the two signals. Here, each signal has its own flip-flop, preserving individual change events.
- **Manual REQ triggering**: Some designs require an external block to generate REQ every time new data arrives. In A2X_CLIENT_2_0, this trigger is generated internally by the flip-flops, reducing the external wiring and keeping the interface compact.

## Conclusion

A2X_CLIENT_2_0 provides a clean, self-contained solution for forwarding two Boolean signals from an A2X adapter to a remote OPC-UA server. By integrating two edge-triggered flip-flops with a standard OPC-UA client, it combines reliable change detection with efficient write operations. Its simple interface—only an INIT event, an adapter socket, and the standard QI/ID/QO/STATUS data—makes it easy to integrate into larger automation networks while ensuring that each state change is transmitted exactly once.
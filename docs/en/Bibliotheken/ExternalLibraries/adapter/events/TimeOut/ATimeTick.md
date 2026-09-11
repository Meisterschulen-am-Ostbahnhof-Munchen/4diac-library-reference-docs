# ATimeTick

![ATimeTick](./ATimeTick.svg)

* * * * * * * * * *
## Introduction
The ATimeTick adapter provides a standardized interface for a time-out service, inspired by the ROOM modeling approach. It enables the coordination between a client and a server for starting, stopping, and confirming time-based operations. The adapter supports both plug and socket roles, facilitating communication between function blocks in an IEC 61499 system.

## Interface Structure
The adapter exposes the following interface elements:

### **Event Inputs**
- **CNF**: Execution confirmation event, triggered when the time-out service completes or acknowledges a request. It carries associated data (Q, ET, PT).
- **STARTO_IN**: Event to start the time-out operation (input).
- **STOPO_IN**: Event to stop the time-out operation (input).

### **Event Outputs**
- **REQ**: Event output that issues a request to the counterpart, typically used to initiate or query the time-out service.

### **Data Inputs**
- **Q** (BOOL): Indicates whether the time-out is currently active (TRUE when started).
- **PT** (TIME): Process time – the configured time-out duration.
- **ET** (TIME): Elapsed time – the time that has already advanced.

### **Data Outputs**
The adapter has no data outputs. All data is passed as inputs, primarily associated with the CNF event.

### **Adapters**
This is an adapter type itself, so it is intended to be used as a plug or socket in a connection. It defines a service interface with two service sequences: "Timeout" and "NormalOperation".

## Functionality
The ATimeTick adapter manages the lifecycle of a time-out operation. It receives start and stop commands (STARTO_IN, STOPO_IN) and provides a confirmation (CNF) after a time-out event. The REQ output serves as a request trigger to the paired service. The data inputs (Q, PT, ET) allow the status and timing parameters to be monitored.

The service definition shows two interaction patterns:
- **Timeout**: A start command (START with parameters TD) is propagated from the plug to the socket. Then, when a time-out occurs, the socket sends a TimeOut event back to the plug.
- **NormalOperation**: Start and stop (START/STOP) events are passed between plug and socket, without an explicit time-out event—likely for normal operation where the time-out is not triggered.

This implies that the adapter can be used in two modes: one where a time-out triggers an output, and another where it simply manages start/stop sequences.

## Technical Features
- **Service Type**: Supports both Plug and Socket roles, enabling bidirectional communication.
- **Event-Driven**: All operations are event-based, ensuring asynchronous coordination.
- **Data Association**: The CNF event is linked with Q, ET, and PT, allowing the confirmation to carry status and timing information.
- **Dynamic Timing**: The PT and ET parameters allow flexible configuration and monitoring of time intervals.
- **Standards Compliance**: Designed according to IEC 61499, with a version history and contributions from multiple organisations.

## State Overview
The adapter does not maintain an internal state machine within its definition; instead, it relies on the event sequences defined in the service. However, the Q data input provides a Boolean flag that can be interpreted as the active/inactive state of the time-out. The transitions are triggered by the events STARTO_IN (start), STOPO_IN (stop), and CNF (time-out occurrence).

## Application Scenarios
- **Timeout Monitoring**: Use the adapter to enforce a time limit on a process. A function block can send STARTO_IN with a desired PT, and the service will respond with CNF after the time expires, allowing the block to react.
- **Process Synchronization**: In normal operation mode, the adapter can coordinate start/stop signals between two function blocks without triggering a time-out, useful for handshake protocols.
- **Error Detection**: By monitoring the ET value, it is possible to detect when a process takes longer than expected.

## Comparison with Similar Blocks
Unlike standard timer function blocks (e.g., TON, TOF) which are self-contained, ATimeTick acts as an interface adapter that separates the timing logic from the requesting block. This promotes reusability and decoupling. Compared to generic adapter types, ATimeTick is specialised for time-out services, providing a clear contract for start/stop and confirmation events.

## Conclusion
The ATimeTick adapter is a valuable component for implementing time-based coordination in IEC 61499 systems. Its clear event and data interface, along with its plug/socket capability, allows for flexible integration into distributed control applications. By following a ROOM-inspired design, it offers a structured approach to managing time-outs and normal operation sequences.
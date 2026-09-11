# logiBUS_QXA_OPC


![logiBUS_QXA_OPC_network](./logiBUS_QXA_OPC_network.svg)

![logiBUS_QXA_OPC](./logiBUS_QXA_OPC.svg)

* * * * * * * * * *

## Introduction

`logiBUS_QXA_OPC` is a composite subapplication designed to provide a generic single‑channel OPC UA read/write access to a logiBUS digital output (DQ) module. It is specifically intended for modules that lack ISOBUS or Virtual Terminal (VT) support, offering a standard OPC UA interface for remote control and monitoring. The subapp internally couples a `logiBUS_QXA` FB with OPC UA subscribe and publish adapters, enabling bidirectional communication over the network.

## Interface Structure

The subapp exposes only data input variables. It does not have any event inputs, event outputs, data outputs, or adapter interfaces at its boundary; all communication is handled internally.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

| Name | Type | Initial Value | Comment |
|------|------|---------------|---------|
| `Output` | `logiBUS::io::DQ::logiBUS_DO_S` | `logiBUS_DO::Invalid` | Identifies the target output channel (e.g., Output_Q1..Q12). |
| `ID_READ` | `WSTRING` | – | OPC UA subscribe key (used to receive commands or status requests). |
| `ID_WRITE` | `WSTRING` | – | OPC UA publish key (used to send data back to the network). |

### **Data Outputs**

None.

### **Adapters**

None at the subapp level. Internally, the following adapter instances are utilised:

- `AX_SUBSCRIBE_1` – subscribes to an OPC UA data source.
- `AX_PUBLISH_1` – publishes data to an OPC UA target.
- `AX_SPLIT_2` – splits the incoming event stream for parallel processing.

## Functionality

The subapp acts as a bridge between an OPC UA client and a physical logiBUS digital output. The external inputs configure the behaviour:

- The `Output` input selects which specific DQ output channel is to be controlled (e.g., Q1…Q12).
- `ID_READ` defines the OPC UA subscribe key – incoming events on this topic are processed.
- `ID_WRITE` defines the OPC UA publish key – outgoing events are sent to this topic.

Internally, the OPC UA subscribe adapter receives events. These events are then split into two paths:

1. The first path goes directly to the `logiBUS_QXA` FB, which interprets the event as a control command and sets the selected output accordingly.
2. The second path is forwarded to the OPC UA publish adapter, allowing the same event to be echoed back or used for acknowledgment/status purposes.

The publish adapter can also transmit status information back to the OPC UA server, enabling read access to the current state of the output. The separation of read and write keys allows for fine‑grained access control.

## Technical Features

- **Single‑channel configuration**: The subapp handles one DQ output at a time, selected via the `Output` input.
- **OPC UA integration**: Uses standard 4diac adapter types `AX_SUBSCRIBE_1` and `AX_PUBLISH_1` for seamless OPC UA connectivity.
- **Event splitting**: `AX_SPLIT_2` ensures that an incoming event can be both processed locally and forwarded to the publishing side without loss.
- **Flexible addressing**: The keys `ID_READ` and `ID_WRITE` are runtime‑writable, allowing dynamic re‑binding to different OPC UA nodes.
- **No VT dependency**: Works with modules that do not implement ISOBUS VT, making it suitable for simpler or custom hardware.

## State Overview

The subapp does not maintain an explicit state machine. The internal FBs (`logiBUS_QXA`, the adapters, and the splitter) operate asynchronously. However, the overall behaviour can be described by the following data flow:

- **Idle**: Waiting for incoming OPC UA events on the subscribe key.
- **Event received**: The subscribe adapter emits an event; the splitter forwards it in parallel to the `logiBUS_QXA` FB (which updates the output) and to the publish adapter (which may send an acknowledgment or status).
- **Publish operation**: The publish adapter sends data on the configured key, which can be used for read‑back of the current output state or for broadcasting the received command.

## Application Scenarios

- Remote control of logiBUS digital outputs via OPC UA in industrial automation environments.
- Integration of logiBUS modules into OPC UA‑based supervisory control and data acquisition (SCADA) systems.
- Use cases where the module has no ISOBUS or VT interface, and a simple OPC UA channel is required.
- Generic deployment across multiple DQ channels by re‑configuring the `Output` input per instance.

## Comparison with Similar Blocks

- **logiBUS_QXA** (the internal FB): This is the raw DQ control block. `logiBUS_QXA_OPC` extends it with OPC UA connectivity and a fixed single‑channel configuration.
- **logiBUS_QXA_VT**: A variant that likely includes VT button and background color support – `logiBUS_QXA_OPC` omits these for modules without VT, reducing complexity.
- **Generic OPC UA I/O blocks**: Many OPC UA adapters exist, but this subapp encapsulates both subscribe and publish directions specifically for logiBUS DQ control, saving integration time.

## Conclusion

`logiBUS_QXA_OPC` provides a compact, reusable solution for integrating logiBUS digital outputs with OPC UA. Its generic single‑channel design makes it adaptable to various modules, while the separation of read and write keys offers flexible access control. The use of standard 4diac adapters ensures easy integration into existing projects. This subapp is ideal for environments where a lightweight, network‑based control path is required.

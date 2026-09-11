# Bool8_TO_logiBUS_QX8


![Bool8_TO_logiBUS_QX8_network](./Bool8_TO_logiBUS_QX8_network.svg)

![Bool8_TO_logiBUS_QX8](./Bool8_TO_logiBUS_QX8.svg)

* * * * * * * * * *

## Introduction

The Bool8_TO_logiBUS_QX8 subapplication provides a generic interface for controlling eight digital output channels on a logiBUS output module. It accepts eight Boolean values (Q_00 to Q_07) and eight channel selector parameters (Output_1 to Output_8), and forwards each Boolean value to its individually selected output channel. All eight channels are triggered simultaneously by a single confirmation (CNF) event, ensuring synchronized updates of the entire 8-bit output group. The subapplication is designed as a reusable building block for any 8-bit output group on logiBUS-based I/O systems.

## Interface Structure

The interface consists of one event input, sixteen data inputs, and no event outputs, data outputs, or adapters.

### **Event Inputs**

| Event | Description |
|-------|-------------|
| CNF | Common trigger (confirmation) event. When this event occurs, all eight internal output FBs are requested simultaneously to apply the currently present Q_00..Q_07 values to the configured output channels. |

### **Event Outputs**

None.

### **Data Inputs**

| Data Input | Type | Initial Value | Description |
|------------|------|---------------|-------------|
| Q_00 | BOOL | – | Logic value for channel 1 (bit position 0). |
| Q_01 | BOOL | – | Logic value for channel 2 (bit position 1). |
| Q_02 | BOOL | – | Logic value for channel 3 (bit position 2). |
| Q_03 | BOOL | – | Logic value for channel 4 (bit position 3). |
| Q_04 | BOOL | – | Logic value for channel 5 (bit position 4). |
| Q_05 | BOOL | – | Logic value for channel 6 (bit position 5). |
| Q_06 | BOOL | – | Logic value for channel 7 (bit position 6). |
| Q_07 | BOOL | – | Logic value for channel 8 (bit position 7). |
| Output_1 | logiBUS::io::DQ::logiBUS_DO_S | logiBUS_DO::Invalid | Channel identifier for the first output position. |
| Output_2 | logiBUS::io::DQ::logiBUS_DO_S | logiBUS_DO::Invalid | Channel identifier for the second output position. |
| Output_3 | logiBUS::io::DQ::logiBUS_DO_S | logiBUS_DO::Invalid | Channel identifier for the third output position. |
| Output_4 | logiBUS::io::DQ::logiBUS_DO_S | logiBUS_DO::Invalid | Channel identifier for the fourth output position. |
| Output_5 | logiBUS::io::DQ::logiBUS_DO_S | logiBUS_DO::Invalid | Channel identifier for the fifth output position. |
| Output_6 | logiBUS::io::DQ::logiBUS_DO_S | logiBUS_DO::Invalid | Channel identifier for the sixth output position. |
| Output_7 | logiBUS::io::DQ::logiBUS_DO_S | logiBUS_DO::Invalid | Channel identifier for the seventh output position. |
| Output_8 | logiBUS::io::DQ::logiBUS_DO_S | logiBUS_DO::Invalid | Channel identifier for the eighth output position. |

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

The subapplication encapsulates eight instances of the `logiBUS::io::DQ::logiBUS_QX` function block. Each instance corresponds to one bit position of an 8-bit output group:

- DigitalOutput_Q1 handles Q_00 and is configured by Output_1.
- DigitalOutput_Q2 handles Q_01 and is configured by Output_2.
- DigitalOutput_Q3 handles Q_02 and is configured by Output_3.
- DigitalOutput_Q4 handles Q_03 and is configured by Output_4.
- DigitalOutput_Q5 handles Q_04 and is configured by Output_5.
- DigitalOutput_Q6 handles Q_05 and is configured by Output_6.
- DigitalOutput_Q7 handles Q_06 and is configured by Output_7.
- DigitalOutput_Q8 handles Q_07 and is configured by Output_8.

All internal FBs have their QI (qualifier/init) parameter permanently set to TRUE, meaning they are always active and ready to process requests.

When the CNF event is received, it is forwarded to the REQ input of every internal FB instance. Each instance then reads the current value of its associated Q_x input and writes it to the physical logiBUS output channel referenced by its Output_x parameter.

Because the event broadcast happens in parallel, all eight channels are updated at the same time, providing a consistent 8-bit output pattern on the bus.

## Technical Features

- **Channel-individual mapping:** Each bit position (Q_00..Q_07) can be routed to any logiBUS digital output channel by configuring the corresponding Output_1..Output_8 parameter. This makes the subapplication independent of fixed hardware wiring.
- **Synchronized triggering:** A single distributed event (CNF) simultaneously activates all eight internal output blocks, guaranteeing that the complete output word is applied in one cycle.
- **Configurable channel identifiers:** The Output_x inputs are of type `logiBUS_DO_S` and default to `logiBUS_DO::Invalid`, forcing the user to explicitly select the desired output channels before operation.
- **Self-contained 8-bit group:** The subapplication encapsulates all control logic, simplifying the reuse of an 8-channel output group in different projects or contexts.
- **No output interface:** Since the result is written directly to the logiBUS hardware, no data outputs are required.

## State Overview

The subapplication itself does not contain an execution control chart (ECC) and therefore has no internal state machine. Its behavior is strictly event-driven: the CNF event triggers the internal `logiBUS_QX` FBs, which may themselves implement their own internal states for handshaking with the logiBUS hardware. From the perspective of the surrounding application, the subapplication behaves deterministically: each CNF event causes a single write operation to the eight configured output channels.

## Application Scenarios

- **Distributed output control:** Use the subapplication in a control system that needs to drive eight independent digital actuators (e.g., relays, valves, signal lamps) over a logiBUS fieldbus segment.
- **Standardized output modules:** When a machine or plant contains several identical 8-bit output groups, the same subapplication can be instantiated multiple times with different channel parameters, reducing engineering effort.
- **Flexible hardware mapping:** In commissioning or reconfiguration situations, the physical output channels can be reassigned by simply changing the Output_x parameters without modifying the logic structure.
- **Combined binary word output:** Use the Q_00..Q_07 inputs to transfer an 8-bit binary word (e.g., a value from a step sequencer or pattern generator) and write it to the bus in one atomic action.

## Comparison with Similar Blocks

| Feature | Bool8_TO_logiBUS_QX8 | Individual logiBUS_QX FBs |
|---------|----------------------|---------------------------|
| Number of channels | 8 (fixed group) | 1 per FB |
| Channel selection | Parameterized per position (Output_1..Output_8) | Single Output parameter per FB |
| Synchronization | All 8 channels triggered by one CNF event | Requires individual REQ events per FB |
| Reusability | Encapsulated, ready-to-use 8-bit group | Requires wiring management by the user |
| Interface complexity | Reduced (only 9 inputs) | Higher when used in groups |

The subapplication adds no functionality beyond the underlying FBs, but it significantly improves the usability, readability, and maintainability of 8-bit output groups.

## Conclusion

Bool8_TO_logiBUS_QX8 is a practical, generic subapplication for controlling an 8-bit digital output group on a logiBUS system. By combining eight individually configurable output channels with a common trigger event, it offers synchronized, hardware-independent output control. Its parameterizable channel mapping and pre-integrated structure reduce engineering overhead and make it a valuable building block for modular automation projects.

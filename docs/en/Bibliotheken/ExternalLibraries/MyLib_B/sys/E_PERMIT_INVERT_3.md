# E_PERMIT_INVERT_3


![E_PERMIT_INVERT_3_network](./E_PERMIT_INVERT_3_network.svg)

![E_PERMIT_INVERT_3](./E_PERMIT_INVERT_3.svg)

* * * * * * * * * *
## Introduction

E_PERMIT_INVERT_3 is a composite subapplication that combines the IEC 61131 boolean inverter F_NOT_BOOL_INIT with the IEC 61499 event gate E_PERMIT_3. It provides three independent event channels that are only enabled when the external PERMIT signal is FALSE. The subapp therefore behaves as a 3-channel inverted event-enable gate: incoming events are forwarded to the outputs as long as the enable condition is de-asserted.

## Interface Structure

### **Event Inputs**

| Name  | Type    | Comment                 |
|-------|---------|-------------------------|
| EI1   | Event   | Event input channel 1   |
| EI2   | Event   | Event input channel 2   |
| EI3   | Event   | Event input channel 3   |

### **Event Outputs**

| Name  | Type    | Comment                  |
|-------|---------|--------------------------|
| EO1   | Event   | Event output channel 1   |
| EO2   | Event   | Event output channel 2   |
| EO3   | Event   | Event output channel 3   |

### **Data Inputs**

| Name   | Type | Comment                           |
|--------|------|-----------------------------------|
| PERMIT | BOOL | Inverted enable condition (Freigabebedingung) |

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

The subapp implements a three-channel event gate with an inverted enable signal. The external PERMIT input is connected to the IN input of F_NOT_BOOL_INIT, whose output is the logical negation of PERMIT. This negated value is then fed to the PERMIT input of E_PERMIT_3.

Inside E_PERMIT_3, an event arriving on EI1, EI2, or EI3 is forwarded to the corresponding output EO1, EO2, or EO3 only while the PERMIT input of the gate is TRUE. Because the subapp inverts the external PERMIT signal before passing it to the gate, the effective forwarding condition for the external user is:

- PERMIT = FALSE → internal permit = TRUE → events are forwarded.
- PERMIT = TRUE → internal permit = FALSE → events are blocked.

No data values are consumed or produced by the subapp; it purely controls event propagation.

## Technical Features

- Reusable composite design built from standard IEC 61131 and IEC 61499 function blocks.
- Three independent event channels controlled by a single shared enable signal.
- Complementary behavior compared to the plain E_PERMIT_3 block.
- F_NOT_BOOL_INIT provides a deterministic boolean negation with a defined initial output state, which avoids undefined behavior during cold start.
- Easy to integrate into larger 4diac-ide applications due to its simple interface.
- No data outputs or adapters reduce wiring complexity.

## State Overview

E_PERMIT_INVERT_3 contains no explicit state machine. Its runtime behavior can be described in terms of the combined states of the internal blocks:

| External PERMIT | F_NOT_BOOL_INIT.OUT | E_PERMIT_3 internal permit | Result                    |
|-----------------|---------------------|----------------------------|---------------------------|
| FALSE           | TRUE                | TRUE                       | Events pass through       |
| TRUE            | FALSE               | FALSE                      | Events blocked            |

There is no event-driven state retention; each incoming event is either forwarded or discarded depending solely on the current value of PERMIT.

## Application Scenarios

- **Safety and emergency-stop logic:** Event processing is permitted only while a safety signal is inactive, e.g. when a stop button is not pressed.
- **Interlock circuits:** Machine event handling is disabled while a guard door is open or a warning condition is active.
- **Supervisory enable/disable:** A central control signal blocks or releases event-driven activities for three parallel subsystems at once.
- **Fail-safe event gating:** In designs where events must be suppressed whenever an alarm flag is set, the inverted permit semantics fit naturally.

## Comparison with Similar Blocks

- **E_PERMIT_3:** The standard E_PERMIT_3 forwards events when its PERMIT input is TRUE. E_PERMIT_INVERT_3 behaves exactly opposite from the perspective of the external signal, making it suitable for active-low enable logic.
- **E_SWITCH / E_DEMUX:** These blocks route events to selected outputs based on a control input, whereas E_PERMIT_INVERT_3 only blocks or forwards events on the same channel.
- **F_NOT_BOOL_INIT:** A pure logic element without event gating semantics. E_PERMIT_INVERT_3 adds the event-gate behavior around it.

## Conclusion

E_PERMIT_INVERT_3 is a compact and practical subapplication for inverted event gating. By internally combining a boolean inverter with a three-channel event permit gate, it offers a clearly named and reusable component for applications that require event flow to be enabled by an active-low signal. Its simple interface, deterministic initialization, and complementary behavior to E_PERMIT_3 make it a valuable building block in IEC 61499-based control systems.
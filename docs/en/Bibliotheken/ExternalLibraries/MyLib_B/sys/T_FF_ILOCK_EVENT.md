# T_FF_ILOCK_EVENT


![T_FF_ILOCK_EVENT_network](./T_FF_ILOCK_EVENT_network.svg)

![T_FF_ILOCK_EVENT](./T_FF_ILOCK_EVENT.svg)

* * * * * * * * * *

## Introduction

T_FF_ILOCK_EVENT is a reusable IEC 61499 subapplication that implements an event-driven toggle flip-flop with an external reset input and a dedicated SET output for interlocking. It is designed for radio-button style mutual exclusion: when one instance becomes active, its SET event can be connected to the RESET input of another instance. Multiple instances can be chained this way to form an interlocked group.

## Interface Structure

### **Event Inputs**

| Name   | Type  | Description                                             |
|--------|-------|---------------------------------------------------------|
| IND   | Event | Toggle trigger. Each event toggles the internal state Q. |
| RESET | Event | Forces Q to FALSE and produces EO.                      |

### **Event Outputs**

| Name | Type  | Description                                                             |
|------|-------|-------------------------------------------------------------------------|
| EO   | Event | Acknowledgment output after an IND or RESET event has been processed.   |
| SET  | Event | Emitted only when Q changes from FALSE to TRUE; used to reset other participants. |

### **Data Inputs**

This subapplication has no data inputs.

### **Data Outputs**

| Name | Type   | Description                                        |
|------|--------|----------------------------------------------------|
| Q    | BOOL   | Current state. TRUE means the participant is active. |

### **Adapters**

This subapplication exposes no adapters.

## Functionality

Internally, T_FF_ILOCK_EVENT uses an E_SR flip-flop and an E_SWITCH event router.

When an IND event arrives, E_SWITCH uses the current state Q to decide whether to set or reset the internal flip-flop:

- If Q = FALSE, the event is routed to the Set input of E_SR. Q becomes TRUE and the SET output is emitted.
- If Q = TRUE, the event is routed to the Reset input of E_SR. Q becomes FALSE and no SET output is emitted.

The external RESET event is connected directly to the Reset input of E_SR. This allows an external event, or the SET output of another participant, to force the block into the inactive state.

After every accepted IND or RESET event, the EO output is emitted.

## Technical Features

- Pure event-driven operation; no polling or cyclic execution is required.
- Uses only standard IEC 61499 event and data connections.
- One BOOL output Q for state indication.
- SET output provides a clean pulse for interlocking or resetting other participants.
- RESET input supports master reset and daisy-chain interlocking.
- No data inputs, so no additional configuration is required.
- Scales to any number of participants by connecting SET of one instance to RESET of the next.

## State Overview

| Current Q | Event | Resulting Q | Output Events |
|-----------|-------|-------------|---------------|
| FALSE     | IND   | TRUE        | SET, EO       |
| TRUE      | IND   | FALSE       | EO            |
| FALSE     | RESET | FALSE       | EO            |
| TRUE      | RESET | FALSE       | EO            |

This behavior creates a toggle switch: each IND toggles the local state, while RESET guarantees a defined inactive state.

## Application Scenarios

- **Radio-button control groups:** Selecting a new participant automatically deselects the previously active one by connecting SET to the previous participant's RESET.
- **Exclusive modes in machines:** Use one instance per mode, e.g. Auto, Manual, Setup, and chain the SET outputs to the other RESET inputs.
- **Toggle pushbuttons:** Use IND from a button to create a latching toggle output, with RESET available for emergency off or master reset.
- **Multi-station interlocking:** Chain any number of instances for arbitration between stations, recipes, or active states.

## Comparison with Similar Blocks

- **E_SR:** A plain set/reset flip-flop requires separate set and reset events. It has no toggle behavior and no interlocking SET output.
- **E_SWITCH:** Routes events based on a selector input but has no memory and cannot store the selected state.
- **A generic T-FF:** Toggles on each input event, but does not provide a dedicated SET output, external reset support, or event-based interlocking convenience.

T_FF_ILOCK_EVENT combines the memory of an E_SR with the routing capability of an E_SWITCH and adds an interlock-oriented SET output, making it especially suitable for event-based radio-button logic.

## Conclusion

T_FF_ILOCK_EVENT is a compact, reusable, event-driven interlock subapplication. It provides toggle behavior, external reset, state output, and a SET pulse for daisy-chaining mutual exclusion groups. Because it requires no data inputs and no additional wiring logic, it can be used consistently in HMI panels, control applications, and multi-station systems.
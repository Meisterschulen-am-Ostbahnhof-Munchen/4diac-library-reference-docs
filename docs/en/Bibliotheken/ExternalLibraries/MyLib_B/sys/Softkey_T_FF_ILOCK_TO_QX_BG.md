# Softkey_T_FF_ILOCK_TO_QX_BG


![Softkey_T_FF_ILOCK_TO_QX_BG_network](./Softkey_T_FF_ILOCK_TO_QX_BG_network.svg)

![Softkey_T_FF_ILOCK_TO_QX_BG](./Softkey_T_FF_ILOCK_TO_QX_BG.svg)

* * * * * * * * * *

## Introduction

Softkey_T_FF_ILOCK_TO_QX_BG is a generic subapplication that implements a digital output driven by a softkey (button) toggle flip-flop. Each released key press toggles the state of a logiBUS_QX digital output. In addition, external SET and RESET event inputs allow overriding or locking the flip-flop independently of the key. The internal background visualization reflects the current output state (green/white), providing an immediate visual feedback in the HMI.

## Interface Structure

### **Event Inputs**

| Name   | Description                                  |
|--------|----------------------------------------------|
| SET    | Sets the internal flip-flop output to TRUE   |
| RESET  | Resets the internal flip-flop output to FALSE|

### **Event Outputs**

None.

### **Data Inputs**

| Name     | Type                          | Initial Value          | Description                                   |
|----------|-------------------------------|------------------------|-----------------------------------------------|
| u16ObjId | UINT                          | ID_NULL                | Object ID of the softkey/button               |
| Output   | logiBUS::io::DQ::logiBUS_DO_S | logiBUS_DO::Invalid    | Digital output configuration (type/address)   |

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

The subapplication combines a softkey input event handler, a Set/Reset flip-flop and a digital output function block:

1. The softkey event block (Softkey_IE) monitors the assigned softkey (identified by `u16ObjId`) and generates an event on every key release (`SK_RELEASED`).
2. The E_SWITCH block routes this event either to the Set input or to the Reset input of the E_SR flip-flop, depending on the current state `Q` (routed to the switch control input `G`):
   - If `Q = FALSE` (`G = FALSE`), the event is routed to `E_SR.S` → the output becomes TRUE.
   - If `Q = TRUE` (`G = TRUE`), the event is routed to `E_SR.R` → the output becomes FALSE.
   This yields a toggle behavior: every softkey release inverts the flip-flop state.
3. The external event inputs `SET` and `RESET` are connected directly to the `S` and `R` inputs of the E_SR flip-flop. They take precedence over the key events and can be used to lock the output to a defined state or to override the toggle.
4. The flip-flop output `Q` drives the `QX` function block, which writes the digital output (e.g., logiBUS output module). The value `Output` (data input) defines the output module and channel configuration.
5. The `GreenWhiteBackground` subapplication is triggered after every completed output write (`QX.CNF`) and receives both the object ID and the current state `Q` as `DI1`, so the background color indicates the output state (green = active, white = inactive).

## Technical Features

- Generic design: works with any softkey object ID and any logiBUS digital output configuration.
- Toggle flip-flop based on edge-triggered softkey release events, debounce handled by the softkey driver.
- External SET/RESET override inputs allow interlocking or forced control.
- Integrated background visualization (green/white) for direct HMI feedback.
- Uses standard 61499 blocks (E_SR, E_SWITCH) and logiBUS function blocks (logiBUS_QX, Softkey_IE).
- Combines event-driven logic with a synchronous data flow for state propagation.

## State Overview

The E_SR flip-flop defines the central state:

| State | Output QX | Background | Trigger to leave state |
|-------|-----------|------------|------------------------|
| FALSE | inactive  | white      | Softkey release (toggle) or external SET |
| TRUE  | active    | green      | Softkey release (toggle) or external RESET |

The E_SWITCH uses the current state `Q` as its switching signal `G`, ensuring the correct event path for the next softkey press.

## Application Scenarios

- **Machine operator panel**: A physical softkey toggles a digital output (e.g., lamp, valve, motor) while a PLC program can lock the output via SET/RESET during safety conditions.
- **Manual/Auto override**: The output can be forced to a defined state by the control system even if the operator presses the key.
- **Visual feedback on HMI**: The background of the corresponding HMI element turns green when the output is active, making the state visible at a glance.
- **Reusable subapplication**: Can be instantiated multiple times for different softkeys and output channels within the same project.

## Comparison with Similar Blocks

| Feature                     | Softkey_T_FF_ILOCK_TO_QX_BG          | Simple Softkey-to-QX (without flip-flop) | Softkey with separate Set/Reset inputs |
|-----------------------------|--------------------------------------|------------------------------------------|----------------------------------------|
| Toggle on key press         | Yes (flip-flop toggles)              | No (output follows key)                  | No                                     |
| External override           | Yes (SET/RESET events)               | No                                       | Yes                                    |
| Background visualization    | Yes (green/white)                    | Optional                                 | Optional                               |
| Generic object ID           | Yes (`u16ObjId`)                     | Yes                                      | Yes                                    |

## Conclusion

Softkey_T_FF_ILOCK_TO_QX_BG is a versatile, generic subapplication that combines a toggle flip-flop softkey logic with external override capability and integrated background visualization. It offers a clean separation between operator input, interlocking logic, and output driving, making it suitable for a wide range of control and visualization tasks in automation systems based on 4diac/logiBUS.

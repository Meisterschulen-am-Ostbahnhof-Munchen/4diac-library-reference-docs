# AUI_AUI_MUX_3_VAL


![AUI_AUI_MUX_3_VAL_network](./AUI_AUI_MUX_3_VAL_network.svg)

![AUI_AUI_MUX_3_VAL](./AUI_AUI_MUX_3_VAL.svg)

* * * * * * * * * *

## Introduction

The **AUI_AUI_MUX_3_VAL** is a composite subapplication that implements a 3‑way multiplexer for AUI (Asynchronous Unified Interface) values. It allows selecting one of three AUI input channels via event triggers, while also providing the ability to set initial output values using standard UINT data inputs. The subapplication encapsulates the logic of a dedicated AUI multiplexer and three initial‑value adapters, simplifying the integration of AUI‑based selection logic into larger systems.

## Interface Structure

### **Event Inputs**

- **EI1** – Event to select the first input channel (val1).  
- **EI2** – Event to select the second input channel (val2).  
- **EI3** – Event to select the third input channel (val3).

Each event triggers the multiplexer to route the corresponding AUI value to the output.

### **Event Outputs**

No explicit event outputs are defined. The subapplication operates purely on event‑driven selection; the result is reflected on the AUI output adapter.

### **Data Inputs**

- **val1** (UINT) – The initial value for the first AUI channel.  
- **val2** (UINT) – The initial value for the second AUI channel.  
- **val3** (UINT) – The initial value for the third AUI channel.

These values are converted to AUI format internally and serve as the default outputs for each corresponding selection event.

### **Data Outputs**

No direct data outputs are exposed. The result is delivered via the AUI output adapter.

### **Adapters**

- **OUT** (AUI, unidirectional) – The selected AUI output. This adapter carries the active channel’s value after an event is received.

## Functionality

The subapplication combines three main internal components:

1. **AUI_MUX_3** – An event‑driven multiplexer control block that accepts the three event inputs (EI1, EI2, EI3) and generates a single control signal (K) indicating which channel is active.  
2. **initval_AUI (×3)** – Three instances that accept a UINT initial value (`INIT_VAL`) and convert it into an AUI‑compatible output (OUT). Each instance is fed with one of the `val1..val3` inputs.  
3. **AUI_AUI_MUX_3** – The core AUI multiplexer that receives three AUI inputs (IN1, IN2, IN3) and the control signal (K). Depending on the control value, it selects one of the inputs and forwards it to the output (OUT).

The internal flow is as follows:

- The UINT values (`val1`, `val2`, `val3`) are passed to the respective `initval_AUI` blocks, which convert them into AUI signals.  
- The event inputs (`EI1`, `EI2`, `EI3`) are fed into `AUI_MUX_3`, which produces a control signal based on which event was triggered.  
- The control signal and the three AUI inputs are routed to `AUI_AUI_MUX_3`, which selects the appropriate channel and passes it to the external `OUT` adapter.

Thus, upon receiving an event, the subapplication immediately outputs the AUI representation of the corresponding `valN` value.

## Technical Features

- **Modular design** – Reuses standard AUI and initval blocks, ensuring interoperability and ease of maintenance.  
- **Event‑driven selection** – No continuous polling; output changes only when one of the three events occurs.  
- **Initial value support** – The UINT inputs allow the subapplication to be initialised with specific values that are converted and made available as AUI signals.  
- **Unidirectional AUI output** – The OUT adapter is unidirectional, suitable for simple point‑to‑point data transfer.  
- **Standardised interfaces** – All inputs and outputs adhere to the 61499‑2 standard, making the block compatible with other IEC 61499‑compliant tools.

## State Overview

The subapplication does not maintain internal state beyond the current selection. It behaves as a combinational selector once an event is received:

- **Idle state** – No event has occurred; output holds its last value.  
- **Selection states** – After EI1, EI2, or EI3, the output reflects the corresponding AUI value.  

There is no explicit reset or default selection; the output remains on the last selected channel until a new event arrives.

## Application Scenarios

- **Process control** – Selecting one of several sensor values (converted to AUI) based on external trigger events.  
- **User‑interface routing** – Choosing between different data streams (e.g., from different sources) based on operator inputs.  
- **Initialization and failover** – Providing a pre‑defined default value on each channel, with the ability to switch quickly when events occur.  
- **System diagnostics** – Multiplexing diagnostic data from multiple subsystems into a single monitoring point.

## Comparison with Similar Blocks

Compared to a simple multiplexer that directly takes UINT inputs and outputs a UINT, this block introduces AUI conversion and event‑driven behaviour. It differs from a basic selector in that it:

- Operates on AUI adapters rather than raw data types.  
- Includes explicit initial value handling via `initval_AUI`, allowing each channel to have a predefined starting value.  
- Is fully event‑driven, whereas some multiplexers may use continuous data inputs or a selection input.

When compared to a standalone `AUI_MUX_3` block, this subapplication adds the UINT‑to‑AUI initialisation layer, making it easier to use in systems that work with standard integer values while still maintaining an AUI output.

## Conclusion

The **AUI_AUI_MUX_3_VAL** subapplication provides a clean, event‑driven solution for multiplexing AUI signals while supporting UINT‑based initial values. Its modular internal structure leverages standard 4diac blocks, ensuring reliability and compatibility. With its simple interface and flexible configuration, it is well suited for a wide range of industrial automation and control applications where AUI communication and event‑based selection are required.

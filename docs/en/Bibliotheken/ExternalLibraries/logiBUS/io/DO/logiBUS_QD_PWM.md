# logiBUS_QD_PWM

<img width="1848" height="333" alt="image" src="https://github.com/user-attachments/assets/ea4b0496-56de-4eb9-a419-6cd8c9b095bb" />
* * * * * * * * * *
## Introduction

The function block `logiBUS_QD_PWM` is an output service interface function block for double-word output data. It serves as an interface for controlling PWM (pulse-width modulation) outputs via the logiBUS system and enables the control of outputs Q1 to Q8.
![logiBUS_QD_PWM](logiBUS_QD_PWM.svg)

## Interface Structure

### **Event Inputs**

- **INIT**: Service Initialization
- Linked to: QI, PARAMS, Output
- **REQ**: Service Request
- Linked to: QI, OUT

### **Event Outputs**

- **INITO**: Initialization Acknowledgement
- Linked to: QO, STATUS
- **CNF**: Acknowledgement of Requested Service Operation
- Linked to: QO, STATUS

### **Data Inputs**

- **QI** (BOOL): Event Input Qualifier
- **PARAMS** (STRING): Service Parameters
- **OUT** (DWORD): Output Data for the Resource (13-bit raw PWM value `0` to `8191`, corresponding to `0 %` to `100 %` duty cycle, $2^{13} = 8192$ states)
- **Output** (logiBUS_DO_S): Identifies the output Output_Q1..Q8
- Initial Value: `logiBUS_DO::Invalid`

### **Data Outputs**

- **QO** (BOOL): Event Output Qualifier
- **STATUS** (STRING): Service Status

### **Adapters**

No adapter interfaces available.

## Functionality

This function block enables PWM control of outputs via the logiBUS system. During initialization (INIT), the service parameters are configured and the specific output is identified. PWM data (`OUT`, 13-bit value `0` to `8191` inside a DWORD variable) can be sent to the configured output via a REQ request. The block acknowledges both initialization and service requests via the corresponding output events.

## Technical Features

- **13-Bit PWM Resolution**: Uses 13-bit normalization (`0` to `8191`) stored in a `DWORD` data type (`0` = 0% duty cycle, `8191` = 100% duty cycle, scaling factor for ISO-Designer: `0.0122085215480405` or `100 / 8191`).
- Supports up to 8 outputs (Q1-Q8) via output configuration
- String-based parameter configuration for flexible service settings
- Status feedback via STRING variable for detailed error information

## State Overview

The function block has two main states:

1. **Not Initialized**: Block waits for an INIT event
2. **Initialized and Ready**: Block can process REQ requests and output PWM data

## Application Scenarios

- Control of PWM-controlled actuators (motors, heating elements)
- Control of LED lighting with brightness control
- Control of valves with proportional control
- Industrial automation applications with logiBUS hardware

## ⚖️ Comparison with Similar Blocks

Compared to simple digital output blocks, `logiBUS_QD_PWM` offers precise PWM control with 13-bit resolution (`0`–`8191` inside a DWORD). Compared to analog output blocks, it enables direct PWM control without additional conversion.

## 🛠️ Related exercises

- [Uebung_034](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034/)
- [Uebung_034a1_Q1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q1/)
- [Uebung_034a1_Q2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q2/)
- [Uebung_034a1_Q4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q4/)
- [Uebung_034b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034b/)
- [Uebung_152](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_152/)
- [Uebung_153](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_153/)

## Conclusion

The `logiBUS_QD_PWM` function block provides a powerful interface for PWM outputs in the logiBUS system. Thanks to its flexible configuration and support for 13-bit PWM data (`0`–`8191`), it is ideally suited for precise control applications in industrial automation systems.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 The PWM signal & infographic on ms-muc-docs.de](https://www.ms-muc-docs.de/automatisierung/das-pwm-signal-die-kunst-spannung-zu-zerhacken/das-pwm-signal-die-kunst-spannung-zu-zerhacken-website/)

]

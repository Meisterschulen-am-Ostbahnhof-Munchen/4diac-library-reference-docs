# logiBUS_IE

<img width="2037" height="313" alt="image" src="https://github.com/user-attachments/assets/3cb31c53-ac68-4483-bfc4-834776122b60" />
* * * * * * * * * *
## Introduction

The logiBUS_IE function block is an input service interface module for event input data. It serves as an interface between the logiBUS system and the 4diac runtime environment for processing digital input signals with extended event detection functions.
![logiBUS_IE](logiBUS_IE.svg)

## Interface Structure

### **Event Inputs**

- **INIT**: Service Initialization
- Linked to: QI, PARAMS, Input, InputEvent
- **REQ**: Service Request
- Linked to: QI

### **Event Outputs**

- **INITO**: Initialization Acknowledgement
- Linked to: QO, STATUS
- **CNF**: Acknowledgement of Requested Services
- Linked to: QO, STATUS
- **IND**: Resource Indication
- Linked to: QO, STATUS

### **Data Inputs**

- **QI** (BOOL): Event Input Qualifier
- **PARAMS** (STRING): Service Parameter
- **Input** (logiBUS_DI_S): Identifies the Input (I1..I8)
- Initial Value: Invalid
- **InputEvent** (logiBUS_DI_Events_S): Identifies the event (Down, Up, Single-Click, Double-Click, etc.)
- Initial value: Invalid

### **Data Outputs**

- **QO** (BOOL): Event output qualifier
- **STATUS** (STRING): Service status

### **Adapter**

No adapter interfaces available.

## Functionality

This function block enables the processing of digital input signals with advanced event detection. Specific parameters can be configured via the INIT initialization. The block recognizes various input events such as key presses, releases, single clicks, and double clicks and signals these via the corresponding output events.

## Technical Features

- Supports multiple event types (Down, Up, Single-Click, Double-Click)
- Flexible input configuration via PARAMS string
- Status feedback via STATUS output
- Initialization with invalid values for a safe start

## State Overview

The function block has an initialized state and an operating state. After successful INIT initialization, it switches to the operating state, in which event processing and service requests can be handled.

## Application Scenarios

- Operator Panel Controllers
- Keyboard Input Processing
- Mechanical Switch Monitoring
- User Interaction Systems
- Industrial Control Pushbuttons

## ⚖️ Comparison with Similar Function Blocks

Compared to simple digital input blocks, logiBUS_IE offers advanced event detection capabilities and can recognize complex input patterns such as double-clicks. The configurable design allows for flexible adaptation to various hardware interfaces.

## 🛠️ Related exercises

- [Uebung_004a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a/)
- [Uebung_004a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a2/)
- [Uebung_004a2_2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a2_2/)
- [Uebung_004a2_3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a2_3/)
- [Uebung_004a2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a2_AX/)
- [Uebung_004a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a3/)
- [Uebung_004a3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a3_AX/)
- [Uebung_004a4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a4/)
- [Uebung_004a4_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a4_AX/)
- [Uebung_004a5](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a5/)
- [Uebung_004a5_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a5_AX/)
- [Uebung_004a6](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a6/)
- [Uebung_004a6_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a6_AX/)
- [Uebung_004a7](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a7/)
- [Uebung_004a7_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a7_AX/)
- [Uebung_004a8](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a8/)
- [Uebung_004a8_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a8_AX/)
- [Uebung_004a9](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a9/)
- [Uebung_004a9_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a9_AX/)
- [Uebung_004a_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a_AX/)
- [Uebung_004b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b/)
- [Uebung_004b2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b2/)
- [Uebung_004b3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b3/)
- [Uebung_004b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX/)
- [Uebung_004b_AX_ASR](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX_ASR/)
- [Uebung_004b_AX_ASR_X](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX_ASR_X/)
- [Uebung_004c1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c1/)
- [Uebung_004c1_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c1_AX/)
- [Uebung_004c2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c2/)
- [Uebung_004c2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c2_AX/)
- [Uebung_004c3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c3/)
- [Uebung_004c3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c3_AX/)
- [Uebung_004c4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c4/)
- [Uebung_004c4_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c4_AX/)
- [Uebung_004c5](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c5/)
- [Uebung_004c5_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c5_AX/)
- [Uebung_006](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006/)
- [Uebung_006_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006_AX/)
- [Uebung_006a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a/)
- [Uebung_006a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a2/)
- [Uebung_006a2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a2_AX/)
- [Uebung_006a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a3/)
- [Uebung_006a3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a3_AX/)
- [Uebung_006a4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a4/)
- [Uebung_006a4_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a4_AX/)
- [Uebung_006a_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a_AX/)
- [Uebung_006b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006b/)
- [Uebung_006b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006b_AX/)
- [Uebung_006d](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006d/)
- [Uebung_006d_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006d_AX/)
- [Uebung_007a1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_007a1/)
- [Uebung_007a1_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a1_AX/)
- [Uebung_007a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_007a2/)
- [Uebung_007a2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a2_AX/)
- [Uebung_007a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_007a3/)
- [Uebung_007a3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a3_AX/)
- [Uebung_009a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_009a/)
- [Uebung_019](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019/)
- [Uebung_019a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019a/)
- [Uebung_019b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019b/)
- [Uebung_019c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019c/)
- [Uebung_020f3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020f3/)
- [Uebung_020f3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020f3_AX/)
- [Uebung_020h](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020h/)
- [Uebung_020h_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020h_AX/)
- [Uebung_020i](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020i/)
- [Uebung_020i_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020i_AX/)
- [Uebung_031](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_031/)
- [Uebung_034b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034b/)
- [Uebung_035](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035/)
- [Uebung_035a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035a/)
- [Uebung_035a1_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a1_AX/)
- [Uebung_035a1b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a1b_AX/)
- [Uebung_035a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035a2/)
- [Uebung_035a2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a2_AX/)
- [Uebung_035a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035a3/)
- [Uebung_035a3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a3_AX/)
- [Uebung_035b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035b/)
- [Uebung_035c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035c/)
- [Uebung_036](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_036/)
- [Uebung_037](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_037/)
- [Uebung_038](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_038/)
- [Uebung_038_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_038_AX/)
- [Uebung_039](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039/)
- [Uebung_039a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039a/)
- [Uebung_040](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_040/)
- [Uebung_040_2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_040_2/)
- [Uebung_040_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_040_AX/)
- [Uebung_041](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_041/)
- [Uebung_042](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_042/)
- [Uebung_043](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_043/)
- [Uebung_080](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_080/)
- [Uebung_080b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_080b/)
- [Uebung_080c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_080c/)
- [Uebung_081](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_081/)
- [Uebung_082](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_082/)
- [Uebung_083](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_083/)
- [Uebung_083_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_083_AX/)
- [Uebung_084](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_084/)
- [Uebung_085](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_085/)
- [Uebung_087](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087/)
- [Uebung_087a1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087a1/)
- [Uebung_091](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_091/)
- [Uebung_093](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_093/)
- [Uebung_093b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_093b/)
- [Uebung_094](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_094/)
- [Uebung_094a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_094a/)
- [Uebung_094a_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094a_AX/)
- [Uebung_095](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_095/)
- [Uebung_095_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_095_AX/)
- [Uebung_110](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_110/)
- [Uebung_111](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_111/)
- [Uebung_124](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_124/)
- [Uebung_127](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_127/)
- [Uebung_128](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_128/)
- [Uebung_128b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_128b/)
- [Uebung_132](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_132/)
- [Uebung_150_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_150_AX/)
- [Uebung_151_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_151_AX/)
- [Uebung_152](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_152/)
- [Uebung_153](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_153/)
- [Uebung_160b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_160b/)
- [Uebung_160b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_160b_AX/)
- [Uebung_171_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_171_AX/)
- [Uebung_179](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_179/)
- [Uebung_180](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_180/)

## Conclusion

The logiBUS_IE function block provides a powerful interface for digital input devices Advanced event detection is available. Its flexible configurability and support for various event types make it ideal for applications requiring complex user interactions.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

]

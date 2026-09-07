# QX

![QX (Output Bit) block interface](https://user-images.githubusercontent.com/69573151/210781265-4dabab2d-a9e3-4da6-a14b-8df0a8ed36e5.png)

* * * * * * * * * *

## Introduction

The QX function block is an output service interface function block for Boolean output data. It serves as an interface between the control logic and physical output devices and enables the output of digital signals to external resources.
![QX](QX.svg)

## Interface Structure

### **Event Inputs**

- **INIT**: Service Initialization Event
- **REQ**: Service Request Event

### **Event Outputs**

- **INITO**: Initialization Acknowledgement
- **CNF**: Acknowledgement of Requested Service Operation

### **Data Inputs**

- **QI** (BOOL): Event Input Qualifier
- **PARAMS** (STRING): Service Parameters for Configuration
- **OUT** (BOOL): Output Data for the Resource

### **Data Outputs**

- **QO** (BOOL): Event Output Qualifier
- **STATUS** (STRING): Service Status Information

### **Adapters**

No adapter interfaces are available.

## Functionality

The QX block processes two main operations: initialization and service requests. The INIT event initializes the service with the specified parameters. The REQ event outputs the Boolean OUT value to the connected resource. Each operation generates a corresponding acknowledgment (INITO or CNF) containing status information.

The INIT event initializes the service with the specified parameters.

## Technical Features

- Specialized for Boolean output data
- Supports configurable service configuration
- Provides detailed status information via the STRING output STATUS
- Uses qualifiers (QI/QO) for event control

## State Overview

The block cycles through the following states:

1. **Not Initialized**: Before the first INIT operation
2. **Initialized**: After successful INIT operation, ready for REQ operations
3. **Active**: During the processing of REQ operations

## Application Scenarios

- Control of digital outputs (relays, LEDs, valves)
- Interface to actuators in automation systems
- Integration into I/O subsystems for distributed controllers
- Test and simulation environments for output signals

## ⚖️ Comparison with Similar Blocks

Compared to generic output blocks, QX offers specific service interface functionality with configurable settings and detailed status reporting. Other blocks, such as simple BOOL outputs, typically have fewer configuration options and less status information.

## 🛠️ Related exercises

- [Uebung_001](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_001/)
- [Uebung_001c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_001c/)
- [Uebung_002](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002/)
- [Uebung_002a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a/)
- [Uebung_002a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a2/)
- [Uebung_002a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a3/)
- [Uebung_002a4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a4/)
- [Uebung_002a5b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a5b/)
- [Uebung_002b2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002b2/)
- [Uebung_002b3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002b3/)
- [Uebung_003](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003/)
- [Uebung_003a0](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003a0/)
- [Uebung_003a_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003a_sub/)
- [Uebung_003b2_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003b2_sub/)
- [Uebung_003b_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003b_sub/)
- [Uebung_003c_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003c_sub/)
- [Uebung_003d](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003d/)
- [Uebung_004a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a/)
- [Uebung_004a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a2/)
- [Uebung_004a2_2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a2_2/)
- [Uebung_004a2_3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a2_3/)
- [Uebung_004a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a3/)
- [Uebung_004a4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a4/)
- [Uebung_004a5](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a5/)
- [Uebung_004a6](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a6/)
- [Uebung_004a7](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a7/)
- [Uebung_004a8](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a8/)
- [Uebung_004a9](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a9/)
- [Uebung_004b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b/)
- [Uebung_004b2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b2/)
- [Uebung_004b3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b3/)
- [Uebung_004c1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c1/)
- [Uebung_004c2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c2/)
- [Uebung_004c3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c3/)
- [Uebung_004c4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c4/)
- [Uebung_004c5](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c5/)
- [Uebung_004c6](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c6/)
- [Uebung_004c7](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c7/)
- [Uebung_005](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_005/)
- [Uebung_006](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006/)
- [Uebung_006a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a/)
- [Uebung_006a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a2/)
- [Uebung_006a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a3/)
- [Uebung_006a4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a4/)
- [Uebung_006b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006b/)
- [Uebung_006c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006c/)
- [Uebung_006d](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006d/)
- [Uebung_006e1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006e1/)
- [Uebung_006e2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006e2/)
- [Uebung_007](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_007/)
- [Uebung_007a1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_007a1/)
- [Uebung_007a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_007a2/)
- [Uebung_007a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_007a3/)
- [Uebung_008](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_008/)
- [Uebung_009](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_009/)
- [Uebung_010](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010/)
- [Uebung_010a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010a/)
- [Uebung_010a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010a2/)
- [Uebung_010a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010a3/)
- [Uebung_010a4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010a4/)
- [Uebung_010b1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b1/)
- [Uebung_010b2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b2/)
- [Uebung_010b3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b3/)
- [Uebung_010b4_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b4_sub/)
- [Uebung_010b5_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b5_sub/)
- [Uebung_010b6](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b6/)
- [Uebung_010b7](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b7/)
- [Uebung_010b8](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b8/)
- [Uebung_010b9](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b9/)
- [Uebung_010bA](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010bA/)
- [Uebung_010bA2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010bA2/)
- [Uebung_010bA3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010bA3/)
- [Uebung_010bA4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010bA4/)
- [Uebung_010c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010c/)
- [Uebung_010c2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010c2/)
- [Uebung_010c3_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010c3_sub/)
- [Uebung_010c4_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010c4_sub/)
- [Uebung_013](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_013/)
- [Uebung_019b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019b/)
- [Uebung_019c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019c/)
- [Uebung_020a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020a/)
- [Uebung_020b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020b/)
- [Uebung_020c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020c/)
- [Uebung_020c2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020c2/)
- [Uebung_020c3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020c3/)
- [Uebung_020d](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020d/)
- [Uebung_020e](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020e/)
- [Uebung_020e2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020e2/)
- [Uebung_020f](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020f/)
- [Uebung_020f2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020f2/)
- [Uebung_020f3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020f3/)
- [Uebung_020g](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020g/)
- [Uebung_020h](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020h/)
- [Uebung_020i](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020i/)
- [Uebung_021](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_021/)
- [Uebung_022](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_022/)
- [Uebung_023](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_023/)
- [Uebung_024](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_024/)
- [Uebung_025](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_025/)
- [Uebung_026_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_026_sub/)
- [Uebung_028](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_028/)
- [Uebung_029](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_029/)
- [Uebung_030](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_030/)
- [Uebung_032](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_032/)
- [Uebung_033_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_033_sub/)
- [Uebung_035](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035/)
- [Uebung_035a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035a/)
- [Uebung_035a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035a2/)
- [Uebung_035a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035a3/)
- [Uebung_035b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035b/)
- [Uebung_035c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035c/)
- [Uebung_036](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_036/)
- [Uebung_037](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_037/)
- [Uebung_038](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_038/)
- [Uebung_039_sub_Outputs](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039_sub_Outputs/)
- [Uebung_039a_sub_Outputs](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039a_sub_Outputs/)
- [Uebung_039b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039b/)
- [Uebung_040](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_040/)
- [Uebung_040_2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_040_2/)
- [Uebung_041](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_041/)
- [Uebung_049](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_049/)
- [Uebung_051](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_051/)
- [Uebung_052](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_052/)
- [Uebung_053](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_053/)
- [Uebung_054](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_054/)
- [Uebung_055](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_055/)
- [Uebung_056](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_056/)
- [Uebung_060_sub_Outputs](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_060_sub_Outputs/)
- [Uebung_071](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_071/)
- [Uebung_071a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_071a/)
- [Uebung_071b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_071b/)
- [Uebung_072b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_072b/)
- [Uebung_080](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_080/)
- [Uebung_080b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_080b/)
- [Uebung_080c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_080c/)
- [Uebung_081](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_081/)
- [Uebung_082](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_082/)
- [Uebung_083](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_083/)
- [Uebung_084](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_084/)
- [Uebung_085](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_085/)
- [Uebung_087](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087/)
- [Uebung_087a1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087a1/)
- [Uebung_087a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087a2/)
- [Uebung_088](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_088/)
- [Uebung_089](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_089/)
- [Uebung_090a1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_090a1/)
- [Uebung_090a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_090a2/)
- [Uebung_091](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_091/)
- [Uebung_093](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_093/)
- [Uebung_093b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_093b/)
- [Uebung_094](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_094/)
- [Uebung_094a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_094a/)
- [Uebung_095](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_095/)
- [Uebung_110](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_110/)
- [Uebung_111](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_111/)
- [Uebung_160](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_160/)
- [Uebung_160b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_160b/)
- [Uebung_160b2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_160b2/)
- [Uebung_177](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_177/)
- [Uebung_178](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_178/)
- [Uebung_179](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_179/)
- [Exercise_180](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_180/)

## Conclusion

The QX function block provides a robust and configurable solution for Boolean output services. Its structured event handling and detailed status feedback make it particularly suitable for reliable automation applications where transparency regarding the I/O status is required.

# logiBUS_QXA

<img width="2042" height="360" alt="image" src="https://github.com/user-attachments/assets/a209d37d-5012-4889-853b-e7a36dfc6644" />
* * * * * * * * * *
## Introduction

The logiBUS_QXA is a composite function block for outputting Boolean data. It serves as an interface for digital output functions and enables the control of up to 8 digital outputs via a standardized protocol.
![logiBUS_QXA](logiBUS_QXA.svg)

## Interface Structure

### **Event Inputs**

- **INIT**: Service initialization event with the associated data QI, PARAMS, and Output

### **Event Outputs**

- **INITO**: Initialization acknowledgment with the data QO and STATUS
- **CNF**: Acknowledgement of the requested service with the data QO and STATUS

### **Data Inputs**

- **QI**: Boolean event input qualifier
- **PARAMS**: Service parameters as a string
- **Output**: Identification of outputs Q1 to Q8 of type logiBUS_DO_S with initial value "Invalid"

### **Data Outputs**

- **QO**: Boolean event output qualifier
- **STATUS**: Service status as a string

### **Adapters**

- **OUT**: Unidirectional adapter of type AX For output communication

## Functionality

The logiBUS_QXA acts as a wrapper for the logiBUS_QX function block and provides a unified interface for digital output functions. During initialization (INIT event), the configuration parameters are passed and the outputs are configured accordingly. The block enables the control of up to 8 digital outputs via the output data structure.

## Technical Features

- Uses the logiBUS_QX core function block for the actual output logic
- Supports up to 8 digital outputs (Q1 to Q8)
- Initialization with specific parameters via PARAMS input
- Feedback on the operating status via STATUS output

## State Overview

The function block goes through the following states:

1. **Not Initialized**: Before the INIT event
2. **Initialization**: During processing of the INIT event
3. **Ready for Operation**: After successful initialization (INITO confirmation)
4. **Active Operation**: Processing of output requests via the OUT adapter

## Application Scenarios

- Control of digital actuators in automation systems
- Connection of output modules in distributed control systems
- Integration into logiBUS-based control architectures
- Industrial automation with Boolean output signals

## ⚖️ Comparison with Similar Blocks

Compared to simple logiBUS_QXA offers the following features for digital output blocks:

- Extended parameterization options
- Status feedback for fault diagnosis
- Standardized interface via adapter
- Support for multiple outputs in a single structure

## 🛠️ Related Exercises

- [Exercise_001_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_001_AX/)
- [Exercise_001_AX_b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_001_AX_b/)
- [Exercise_001c_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_001c_AX/)
- [Exercise_002_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002_AX/)
- [Exercise_002a2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a2_AX/)
- [Exercise_002a3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a3_AX/)
- [Uebung_002a5_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a5_AX/)
- [Uebung_002a5b_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a5b_AX/)
- [Uebung_002a6_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a6_AX/)
- [Uebung_002a7_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a7_AX/)
- [Uebung_002a_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a_AX/)
- [Uebung_002b3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002b3_AX/)
- [Uebung_003_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_003_AX/)
- [Uebung_003a0_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_003a0_AX/)
- [Uebung_003a_AX_sub](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_003a_AX_sub/)
- [Uebung_003d_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_003d_AX/)
- [Uebung_004a2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a2_AX/)
- [Uebung_004a3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a3_AX/)
- [Uebung_004a4_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a4_AX/)
- [Uebung_004a5_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a5_AX/)
- [Uebung_004a6_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a6_AX/)
- [Uebung_004a7_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a7_AX/)
- [Uebung_004a8_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a8_AX/)
- [Uebung_004a9_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a9_AX/)
- [Uebung_004a_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a_AX/)
- [Uebung_004b_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX/)
- [Uebung_004b_AX_ASR](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX_ASR/)
- [Uebung_004b_AX_ASR_X](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX_ASR_X/)
- [Uebung_004c1_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c1_AX/)
- [Uebung_004c2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c2_AX/)
- [Uebung_004c3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c3_AX/)
- [Uebung_004c4_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c4_AX/)
- [Uebung_004c5_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c5_AX/)
- [Uebung_004c6_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c6_AX/)
- [Uebung_004c7_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c7_AX/)
- [Uebung_005_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_005_AX/)
- [Uebung_006_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006_AX/)
- [Uebung_006a2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a2_AX/)
- [Uebung_006a3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a3_AX/)
- [Uebung_006a4_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a4_AX/)
- [Uebung_006a_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a_AX/)
- [Uebung_006b_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006b_AX/)
- [Uebung_006d_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006d_AX/)
- [Uebung_006e1_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006e1_AX/)
- [Uebung_006e2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006e2_AX/)
- [Uebung_007_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007_AX/)
- [Uebung_007a1_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a1_AX/)
- [Uebung_007a2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a2_AX/)
- [Uebung_007a3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a3_AX/)
- [Uebung_008_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_008_AX/)
- [Uebung_009_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_009_AX/)
- [Uebung_010_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010_AX/)
- [Uebung_010a2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010a2_AX/)
- [Uebung_010a3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010a3_AX/)
- [Uebung_010a_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010a_AX/)
- [Uebung_010b1_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b1_AX/)
- [Uebung_010b2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b2_AX/)
- [Uebung_010b3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b3_AX/)
- [Uebung_010b4_sub_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b4_sub_AX/)
- [Uebung_010b5_sub_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b5_sub_AX/)
- [Uebung_010b6_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b6_AX/)
- [Uebung_010b7_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b7_AX/)
- [Uebung_010b8_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b8_AX/)
- [Uebung_010b9_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b9_AX/)
- [Uebung_010bA2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA2_AX/)
- [Uebung_010bA3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA3_AX/)
- [Uebung_010bA4_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA4_AX/)
- [Uebung_010bA_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA_AX/)
- [Uebung_010c2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010c2_AX/)
- [Uebung_010c3_sub_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010c3_sub_AX/)
- [Uebung_010c4_sub_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010c4_sub_AX/)
- [Uebung_010c_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010c_AX/)
- [Uebung_013_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_013_AX/)
- [Uebung_020a_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020a_AX/)
- [Uebung_020b_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020b_AX/)
- [Uebung_020c3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020c3_AX/)
- [Uebung_020c_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020c_AX/)
- [Uebung_020d_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020d_AX/)
- [Uebung_020e2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020e2_AX/)
- [Uebung_020e_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020e_AX/)
- [Uebung_020f2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020f2_AX/)
- [Uebung_020f3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020f3_AX/)
- [Uebung_020f_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020f_AX/)
- [Uebung_020g_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020g_AX/)
- [Uebung_020h_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020h_AX/)
- [Uebung_020i_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020i_AX/)
- [Uebung_020j2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020j2_AX/)
- [Uebung_020j_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020j_AX/)
- [Uebung_035a1_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a1_AX/)
- [Uebung_035a1b_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a1b_AX/)
- [Uebung_035a2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a2_AX/)
- [Uebung_035a3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a3_AX/)
- [Uebung_038_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_038_AX/)
- [Uebung_040_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_040_AX/)
- [Uebung_083_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_083_AX/)
- [Uebung_090a1_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_090a1_AX/)
- [Uebung_090a2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_090a2_AX/)
- [Uebung_094a_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094a_AX/)
- [Uebung_095_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_095_AX/)
- [Uebung_103](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_103/)
- [Uebung_103c](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_103c/)
- [Uebung_103c2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_103c2/)
- [Uebung_150_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_150_AX/)
- [Uebung_151_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_151_AX/)
- [Uebung_160_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_160_AX/)
- [Uebung_160b2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_160b2_AX/)
- [Uebung_160b_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_160b_AX/)
- [Uebung_171_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_171_AX/)
- [Exercise_177_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_177_AX/)
- [Exercise_178_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_178_AX/)

## Conclusion

The logiBUS_QXA is a robust and flexible function block for digital output functions in industrial automation systems. Its standardized interface and comprehensive parameterization options make it ideally suited for use in complex control architectures with high demands on reliability and diagnostic capabilities.

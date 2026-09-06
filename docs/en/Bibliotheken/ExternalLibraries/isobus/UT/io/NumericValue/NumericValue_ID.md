# NumericValue_ID

<img width="1399" height="290" alt="image" src="https://github.com/user-attachments/assets/d47cfe0c-6224-4132-bb02-d186bc0e0c55" />
* * * * * * * * * *
## Introduction

The NumericValue_ID function block is an input service interface function block for double-word input data (DWORD). It serves as an interface for communication with resources and enables the processing of numeric input values in ISOBUS-compatible systems.
![NumericValue_ID](NumericValue_ID.svg)

## Interface Structure

### **Event Inputs**

- **INIT**: Service Initialization
- Linked to: QI, PARAMS, u16ObjId
- **REQ**: Service Request
- Linked to: QI

### **Event Outputs**

- **INITO**: Initialization Acknowledgement
- Linked to: QO, STATUS
- **CNF**: Acknowledgement of Requested Service Request
- Linked to: QO, STATUS, IN
- **IND**: Resource Indication
- Linked to: QO, STATUS, IN

### **Data Inputs**

- **QI** (BOOL): Event Input Qualifier
- **PARAMS** (STRING): Service Parameters
- **u16ObjId** (UINT): Object ID with Initial Value ID_NULL

### **Data Outputs**

- **QO** (BOOL): Event output qualifier
- **STATUS** (STRING): Service status
- **IN** (DWORD): Input data from the resource

### **Adapter**

No adapter interfaces are available.

## Functionality

The function block initializes itself via the INIT event and can make service requests via REQ. Upon successful initialization, it returns INITO, while CNF and IND represent acknowledgments of service requests and indications from the resource, respectively. Data is transmitted via DWORD values (32-bit), which are provided via the IN output.

## Technical Features

- Uses DWORD data type for numeric input values
- ISOBUS-compatible implementation
- Initialization with standardized object ID (ID_NULL)
- String-based status feedback
- Supports configurable service configuration

## State Overview

The function block goes through the following states:

1. **Not Initialized**: Before INIT call
2. **Initialized**: After successful INIT processing
3. **Service Active**: During active REQ requests
4. **Data Provisioning**: During active IND indication

## Application Scenarios

- Agricultural control systems (ISOBUS)
- Industrial automation with numeric sensor data
- Embedded systems with double-word data processing
- Systems with standardized object identifications

## ⚖️ Comparison with Similar Blocks

Compared to simple input blocks, NumericValue_ID offers extended service functionality with initialization procedures, Status feedback and configurable options. Its specific focus on ISOBUS environments distinguishes it from generic input blocks.

## 🛠️ Related exercises

- [Uebung_011](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_011/)
- [Uebung_012](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_012/)
- [Uebung_012a_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_012a_sub/)
- [Uebung_012b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_012b/)
- [Uebung_012c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_012c/)
- [Uebung_020c2_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020c2_sub/)
- [Uebung_034a1_Q1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q1/)
- [Uebung_034a1_Q2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q2/)
- [Exercise_034a1_Q4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q4/)
- [Exercise_103](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_103/)

## Conclusion

The NumericValue_ID function block provides a robust solution for processing double-word input data in ISOBUS-based systems. Its structured initialization, clear status feedback, and flexible parameterization make it ideally suited for industrial applications with high demands on reliability and diagnostic capabilities.

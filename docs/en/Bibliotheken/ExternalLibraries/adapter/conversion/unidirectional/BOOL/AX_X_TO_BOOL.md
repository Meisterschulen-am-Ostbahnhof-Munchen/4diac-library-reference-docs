# AX_X_TO_BOOL

<img width="1243" height="236" alt="image" src="https://github.com/user-attachments/assets/4e98b420-508f-407b-bb14-cfeee976d357" />
* * * * * * * * * *
## Introduction

The AX_X_TO_BOOL function block is a composite function block designed for converting AX data to the BOOL data type. This block acts as an adapter converter and enables unidirectional data conversion between different data types in 4diac systems.
![AX_X_TO_BOOL](AX_X_TO_BOOL.svg)

## Interface Structure

### **Event Inputs**

No direct event inputs are available.

### **Event Outputs**

- **CNF** (Confirmation): Confirmation event output by the adapter.

### **Data Inputs**

No direct data inputs are available.

### **Data Outputs**

- **IN** (BOOL): Converted input data from the adapter.

### **Adapter**

- **AX_IN**: Adapter input of type AX for unidirectional data transmission.

## Functionality

The function block receives data via the AX_IN adapter and converts it to the BOOL data type. Upon successful conversion, the confirmation event CNF is triggered, and the converted data is made available at the IN output. The internal structure directly connects the adapter event output E1 to the CNF output and the adapter's data output D1 to the IN data output.

## Technical Features

- Implemented as a composite function block
- Uses unidirectional adapter communication
- Direct passthrough without additional processing logic
- Simple and efficient data conversion

## State Overview

The function block has a simple state:

- **Ready**: Waits for incoming data from the adapter
- Upon receiving data, conversion and output occur immediately

## Application Scenarios

- Integration of AX-based components into BOOL-oriented controllers
- Adapter conversion in heterogeneous automation systems
- Interface adaptation between different device types
- Protocol conversion in distributed control systems

## ⚖️ Comparison with Similar Function Blocks

Compared to other conversion blocks, AX_X_TO_BOOL stands out due to its specific focus on AX-to-BOOL conversion. While universal converters often require more complex parameterization, this block offers an optimized solution for this specific use case.

## 🛠️ Related exercises

- [Uebung_001_AX_b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_001_AX_b/)
- [Uebung_002a2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a2_AX/)
- [Uebung_004b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX/)
- [Uebung_004b_AX_ASR](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX_ASR/)
- [Uebung_004b_AX_ASR_X](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX_ASR_X/)
- [Uebung_094a_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094a_AX/)

## Conclusion

AX_X_TO_BOOL is a specialized conversion block that enables efficient and reliable conversion of AX data to BOOL values. Its simple structure and direct functionality make it particularly suitable for systems requiring high-performance data type conversion.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

]

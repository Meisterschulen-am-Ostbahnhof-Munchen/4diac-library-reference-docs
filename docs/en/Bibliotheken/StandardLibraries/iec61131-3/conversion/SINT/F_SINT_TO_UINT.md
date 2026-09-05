# F_SINT_TO_UINT

<img width="1441" height="213" alt="F_SINT_TO_UINT" src="https://github.com/user-attachments/assets/76c8c3fe-c626-4192-8647-9b5e552de116" />
* * * * * * * * * *
## Introduction

The function block `F_SINT_TO_UINT` converts a signed 8-bit integer value (`SINT`) into an unsigned 16-bit integer value (`UINT`). This conversion is particularly necessary when exchanging data between systems that use different data types.
![F_SINT_TO_UINT](F_SINT_TO_UINT.svg)

## Interface Structure

### **Event Inputs**

- **REQ**: Starts the conversion. This input is connected to the data input `IN`.

### **Event Outputs**

- **CNF**: Signals the completion of the conversion. The output is connected to the data output `OUT`.

### **Data Inputs**

- **IN**: The input expects a value of type `SINT` (signed 8-bit integer).

### **Data Outputs**

- **OUT**: The output returns the converted value as `UINT` (unsigned 16-bit integer).

### **Adapters**

- No adapters are present.

## Operation

The function block performs the conversion as soon as the event `REQ` is received. The input value `IN`, of type `SINT`, is converted to a value of `UINT` and output `OUT`. Successful conversion is signaled by the event `CNF`.

## Technical Features

- The conversion is performed using the function `SINT_TO_UINT`, which is implemented in the function block's algorithm.
- The function block is simple and requires no additional state management.

## State Overview

Since this is a simple function block, there are no complex states. The block reacts directly to the `REQ` event by executing the conversion and outputting the result.

## Application Scenarios

- Conversion of sensor values stored as `SINT` for systems that expect `UINT`.
- Data preparation for communication protocols that require unsigned values.

## ⚖️ Comparison with Similar Blocks

- Compared to generic conversion blocks, `F_SINT_TO_UINT` is specialized and optimized for converting `SINT` to `UINT`.
- Other blocks, such as `F_INT_TO_UINT` or `F_DINT_TO_UDINT`, offer similar functionality, but for different data types.

## 🛠️ Related exercises

- [Uebung_035](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035/)
- [Uebung_035b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035b/)
- [Uebung_035c](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035c/)
- [Uebung_036](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_036/)
- [Uebung_037](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_037/)
- [Uebung_038](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_038/)
- [Uebung_038_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_038_AX/)
- [Uebung_039_sub_NumbAnsicht](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039_sub_NumbAnzeig/)
- [Uebung_040](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_040/)
- [Exercise_040_2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_040_2/)
- [Exercise_040_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_040_AX/)
- [Exercise_041](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_041/)

## Conclusion

The `F_SINT_TO_UINT` function block is an efficient tool for the specific conversion of signed to unsigned integer values. Its simplicity and direct operation make it a reliable solution in control and automation systems.

# Q_NumericValue

![Q_NumericValue](https://user-images.githubusercontent.com/113907471/204326982-47eea33a-9b9c-4107-8f96-97c85a945fbc.png)

* * * * * * * * * *

## Introduction

The **Q_NumericValue** is a standards-compliant function block for changing numeric values in Virtual Terminals, developed under the EPL-2.0 license. Version 1.0 implements the ISO 11783-6 (Part 6 - F.22) specification for numeric VT objects.
![Q_NumericValue](Q_NumericValue.svg)

## Interface Structure

### **Event Inputs**

- `INIT`: Initialization Request (with object ID)
- `REQ`: Value Change Request

### **Event Outputs**

- `INITO`: Initialization Acknowledgement
- `CNF`: Change Acknowledgement

### **Data Inputs**

- `u16ObjId` (UINT): Object ID (16-bit)
- `u32NewValue` (UDINT): New Numeric Value (32-bit unsigned)

### **Data Outputs**

- `STATUS` (STRING): Operational Status Message
- `u32OldValue` (UDINT): Previous numeric value
- `s16result` (INT): ISO-compliant result code

## Valid Object IDs

**`u16ObjId` — valid object types (Annex F.22, objects with numeric value attribute):**
Input Boolean Field (7000–7999), Input Number Field (9000–9999), Input List Field (10000–10999), Output Number Field (12000–12999), Meter (17000–17999), Linear Bar Graph (18000–18999), Arched Bar Graph (19000–19999), Number Variable (21000–21999), Object Pointer (27000–27999), Output List Object (37000–37999), External Object Pointer (43000–43999), Animation Object (44000–44999), Scaled Graphic Object (48000–48999).

ID_NULL (65535) is not a command target but deactivates the FB when used with `INIT`. Any ID outside these ranges is invalid for commanding.

## Functionality

1. **Initialization**:

- `INIT` with target object ID
- `INITO` confirms operational readiness
1. **Value Update**:

- `REQ` with new 32-bit value
- Updates the numeric VT object
- `CNF` returns operational status and previous value
1. **Value Range**:

- 0 to 4,294,967,295 (32-bit unsigned)

## Technical Features

✔ **ISO 11783-6 compliant** (F.22)

✔ **32-bit value range** (UDINT)

✔ **Instant update**

✔ **Traceability** (Previous value)

✔ **Internal buffering**: The function block buffers the value internally. A message is only sent to the bus if `u32NewValue` differs from `u32OldValue`. This significantly reduces the bus load and tolerates frequent REQ events.

## Value range

| Parameter | Type | Value range |
|-------------|-----------|-----------------------|
| u32NewValue | UDINT | 0 to 4,294,967,295 |

## Return codes (s16result)

| Code | Constant | Meaning |
| ------ | ------------------------- | ------------------------------------ |
| 0 | VT_E_NO_ERR | Successful change |
| -6 | VT_E_OVERFLOW | Buffer overflow |
| -8 | VT_E_NOACT | VT not ready |
| -21 | VT_E_NO_INSTANCE | No VT client available |
| -128 | VT_E_HANDLE_INVALID | Invalid object ID |
| -129 | VT_E_ISO_INSTANCE_INVALID | Invalid VT instance |
| -130 | VT_E_NOT_ALIVE | VT not active |

## Application Scenarios

- **Process Visualization**: Real-time Measurement Data
- **Control Elements**: Setpoint Specifications
- **Diagnostic Systems**: Error Code Display
- **Production Data**: Counters and Statistics

## ⚖️ Comparison with Similar Function Blocks

| Feature | Q_NumericValue | VtNumberUpdate | VtDataManager |
--------------- | ---------------- | ---------------- | --------------- |
| ISO Standard | ✔ | ✖ | ✖ |
| Value Range | 32-bit | 16-bit | 32-bit |
| Feedback | ✔ | ✖ | ✔ |
| Object Type | Numeric | All | All |

## 🛠️ Related exercises

- [Uebung_009](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_009/)
- [Uebung_009a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_009a/)
- [Uebung_011a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_011a/)
- [Uebung_011a2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_011a2/)
- [Uebung_012](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_012/)
- [Uebung_012a_sub](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_012a_sub/)
- [Uebung_012b](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_012b/)
- [Uebung_015](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_015/)
- [Uebung_015a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_015a/)
- [Uebung_020c2_sub](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020c2_sub/)
- [Uebung_035](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035/)
- [Uebung_035b](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035b/)
- [Uebung_035c](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035c/)
- [Uebung_036](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_036/)
- [Uebung_037](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_037/)
- [Uebung_038](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_038/)
- [Uebung_038_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_038_AX/)
- [Uebung_039_sub_NumbAnsicht](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039_sub_NumbAnzeig/)
- [Uebung_040](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_040/)
- [Uebung_040_2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_040_2/)
- [Uebung_040_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_040_AX/)
- [Uebung_041](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_041/)
- [Uebung_070](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_070/)
- [Uebung_071](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_071/)
- [Uebung_071a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_071a/)
- [Uebung_071b](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_071b/)
- [Uebung_072](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_072/)
- [Exercise_072b](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_072b/)
- [Exercise_072c](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_072c/)
- [Exercise_073](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_073/)
- [Exercise_074](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_074/)
- [Exercise_083](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_083/)

## Conclusion

The Q_NumericValue block offers precise numerical control:

- **High-resolution**: 32-bit precision
- **Reliable**: Integrated error checking
- **Flexible**: For all numerical objects

Essential for:

- Precise process visualization
- Real-time data monitoring
- Industrial control systems

## Example applications

[Q_NumericValue_examples](Q_NumericValue_beispiele.md)

# Softkey_IX

## 🎧 Podcast

- [ISO 11783-6: Understanding Softkeys and the Virtual Terminal – Your Key to Agricultural Machinery Mechatronics](https://podcasters.spotify.com/pod/show/isobus-vt-objects/episodes/ISO-11783-6-Softkeys-und-das-Virtual-Terminal-verstehen--Dein-Schlssel-zur-Landmaschinen-Mechatronik-e36a8b0)

## Introduction

The Softkey_IX is a service interface function block for Boolean input data, serving as an input interface for softkey functionalities. This block enables communication with resources and the processing of keyboard input in control systems.

- ![Softkey_IX](Softkey_IX.svg)

## Interface Structure

### **Event Inputs**

- **INIT**: Service Initialization
- Linked to: QI, PARAMS, u16ObjId
- **REQ**: Service Request
- Linked to: QI

### **Event Outputs**

- **INITO**: Initialization Acknowledgement
- Linked to: QO, STATUS
- **CNF**: Acknowledgement of Requested Service Function
- Linked to: QO, STATUS, IN
- **IND**: Indication from Resource
- Linked to: QO, STATUS, IN

### **Data Inputs**

- **QI**: BOOL - Event Input Qualifier
- **PARAMS**: STRING - Service Parameter
- **u16ObjId**: UINT - Object ID (Initial Value: ID_NULL)

### **Data Outputs**

- **QO**: BOOL - Event output qualifier
- **STATUS**: STRING - Service status
- **IN**: BOOL - Input data from the resource

### **Adapters**

No adapter interfaces are available.

## Functionality

The Softkey_IX function block acts as an intermediary between the application logic and physical or virtual input devices. During initialization (INIT), the service parameters are configured. Service requests can be made via REQ events, which are acknowledged by CNF events. IND events signal asynchronous input from the resource.

## Technical Features

- Uses ISOBUS-compatible object identification
- Supports configurable service configuration via STRING parameters
- Offers both synchronous (CNF) and asynchronous (IND) operating modes
- Initialization with a standardized object ID (ID_NULL)

## State Overview

The function block goes through the following main states:

1. **Not Initialized**: Before INIT processing
2. **Initialized**: After successful INIT processing
3. **Ready**: Can process service requests
4. **Active**: Processes current inputs

## Application Scenarios

- Operator panels in mobile machinery
- Virtual keyboards in industrial plants
- Softkey implementations in vehicle systems
- User interfaces for ISOBUS-compatible devices

## ⚖️ Comparison with Similar Blocks

Compared to simple digital input blocks, Softkey_IX offers extended service functionalities with configurable settings and ISOBUS compatibility. The IND functionality enables asynchronous event handling, which is not available with purely query-based function blocks.

## 🛠️ Related exercises

- [Uebung_010](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010/)
- [Uebung_010a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010a/)
- [Uebung_010a4](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010a4/)
- [Uebung_010b4_sub](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b4_sub/)
- [Uebung_010b5_sub](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b5_sub/)
- [Uebung_010c](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010c/)
- [Uebung_010c2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010c2/)
- [Uebung_010c3_sub](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010c3_sub/)
- [Exercise_010c4_sub](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010c4_sub/)
- [Exercise_039_sub_Outputs](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039_sub_Outputs/)
- [Exercise_039b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039b/)

## Conclusion

The Softkey_IX function block represents a flexible and standardized solution for softkey inputs in industrial control systems. Its ISOBUS compatibility and comprehensive service interface make it particularly suitable for demanding applications in mobile machinery and industrial plants.

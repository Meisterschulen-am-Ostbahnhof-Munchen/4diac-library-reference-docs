# CbVtStatus

![CbVtStatus](https://user-images.githubusercontent.com/116869307/214146846-2d9f0694-b5f4-4e70-aae0-dc64540eb356.png)

* * * * * * * * * *

## Introduction

The **CbVtStatus** is a standardized callback module for virtual terminal page changes, developed under the EPL 2.0 license. Version 1.0 supports ISO 11783-6 (ISOBUS) in agricultural tax systems.
![CbVtStatus](CbVtStatus.svg)

## Interface Structure

### **Event Inputs**

- `INIT`: Initialization Request

### **Event Outputs**

- `INITO`: Initialization Confirmation (with STATUS)
- `IND`: Page Change Indication (with page info, `STATUS`, `wPage`, `qWsActive`, `u8Instance`, `wSKM_Scal`, `w_DMScal`, `u8SKM_Number`, `qScalingTestEnabled`)

### **Data Outputs**

- `STATUS` (STRING): Operational Status
- `u8Instance` (USINT): Client Instance Identifier
- `qWsActive` (BOOL): Workload Status
- `wPage` (UINT): Object ID of the open/last mask, or `ID_NULL` (0xFFFF) if unknown
- `wSKM_Scal` (UINT): Softkey mask scaling factor (PoolSoftKeyMaskScalFaktor), unit 1/10000 (10000 = 100%); possibly overridden by SCALING-TEST
- `w_DMScal` (UINT): Data mask scaling factor (PoolDataMaskScalFaktor), unit 1/10000 (10000 = 100%); possibly overridden by SCALING-TEST
- `u8SKM_Number` (USINT): Number of physical softkeys (VT_PHYSICALSOFTKEYS/sknu), valid range 6..11; possibly overridden by SCALING-TEST
- `qScalingTestEnabled` (BOOL): `TRUE` if the SCALING-TEST override (`[ScalingTest] enable=1` in `settings.ini`) is active

## Functionality

1. **Initialization**:

- Event `INIT` starts setup
- Event `INITO` confirms with STATUS

1. **Page Change**:

- Event `IND` is triggered on screen change
- Returns: Page ID, Active Status, Instance Number

1. **Operation**:

- Real-time monitoring of the VT status
- Multi-ECU capable architecture

## Technical Features

✔ **ISO 11783-6 compliant**
✔ **Deterministic** behavior
✔ **Multi-ECU support**
✔ **Plug-in capable** architecture

## Supported Operating Modes

| Mode          | Description                          |
| ------------- | ------------------------------------ |
| Single device | Standard operation with u8Instance=0 |
| Multi-ECU     | Parallel Instances (1-255)           |
| Diagnostics   | STATUS Messages for Service          |

## Application Scenarios

- **Tractor Control Units**: Operator Screen Monitoring
- **Harvesting Machines**: Work Data Display
- **Diagnostic Tools**: VT Status Monitoring
- **Multi-Display Systems**: Synchronization

## ⚖️ Comparison with Similar Modules

| Feature         | CbVtStatus | VtPageManager | VtEventLogger |
| --------------- | ---------- | ------------- | ------------- |
| Page Switch     | ✔          | ✔             | ✖             |
| Multi-ECU       | ✔          | ✖             | ✖             |
| STATUS feedback | ✔          | ✖             | ✔             |

## Error Handling

- Invalid Page ID:
- STATUS = "ERR_INVALID_PAGE"
- wPage = 0xFFFF
- Initialization Error:
- STATUS = "ERR_INIT_FAILED"
- System in Safe State

## 🛠️ Related Exercises

- [Exercise_012](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_012/)
- [Exercise_012a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_012a/)
- [Exercise_012b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_012b/)
- [Exercise_012c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_012c/)

## Conclusion

The CbVtStatus module is essential for ISOBUS-compliant agricultural systems:

- Robust page change management
- Full ISO 11783-6 compatibility
- Proven architecture In series production

Especially valuable for manufacturers of:

- Tractor control units
- Harvesting machine control systems
- ISOBUS diagnostic devices

The standardized implementation enables reliable operation in heterogeneous system environments.

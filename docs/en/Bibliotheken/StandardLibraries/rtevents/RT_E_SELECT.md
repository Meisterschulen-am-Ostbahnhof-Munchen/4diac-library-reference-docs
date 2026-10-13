# RT_E_SELECT

* * * * * * * * * *

## Introduction

Real-time version of the E_SELECT block. Selects one of two input events based on a selector.
![RT_E_SELECT](RT_E_SELECT.svg)

## Interface Structure

### **Event Inputs**

- **INIT**: Initialization, carries `QI`, `Tmin`, `Deadline` and `WCET`.
- **EI0**: Input event, selected when `G` = 0; carries `G`.
- **EI1**: Input event, selected when `G` = 1; carries `G`.

### **Event Outputs**

- **INITO**: Confirms the initialization, carries `QO`.
- **EO**: Output event; carries no data.

### **Data Inputs**

- **QI** (`BOOL`): Qualifier for the initialization.
- **G** (`BOOL`): Selector: `EI0` when `G` = 0, `EI1` when `G` = 1.
- **Tmin** (`TIME`): Real-time parameter (minimum distance).
- **Deadline** (`TIME`): Real-time parameter (deadline).
- **WCET** (`TIME`): Real-time parameter (worst-case execution time).

### **Data Outputs**

- **QO** (`BOOL`): Output qualifier of the initialization.

## Metadata

| Attribute     | Value                            |
| :------------ | :------------------------------- |
| License       | EPL-2.0                          |
| Version       | 3.0 (2025-04-14, Patrick Aigner) |
| 4diac Package | eclipse4diac::rtevents           |

--

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

# RT_E_TRAIN

![RT_E_TRAIN_network](./RT_E_TRAIN_network.svg)

* * * * * * * * * *

## Introduction

Real-time version of the E_TRAIN block. Generates a sequence of events.
![RT_E_TRAIN](RT_E_TRAIN.svg)

## Interface Structure

### **Event Inputs**

- **START**: Starts the event train, carries `DT`, `N`, `Deadline` and `WCET`.
- **STOP**: Stops the event train; carries no data.

### **Event Outputs**

- **EO**: `N` events at period `DT`, starting `DT` after `START`; carries `CV`.

### **Data Inputs**

- **DT** (`TIME`): Period between events.
- **N** (`UINT`): Number of events to generate.
- **Deadline** (`TIME`): Real-time parameter (deadline).
- **WCET** (`TIME`): Real-time parameter (worst-case execution time).

### **Data Outputs**

- **CV** (`UINT`): Index of the current `EO` (0 to N-1).

## Metadata

| Attribute     | Value                            |
| :------------ | :------------------------------- |
| License       | EPL-2.0                          |
| Version       | 3.0 (2025-04-14, Patrick Aigner) |
| 4diac Package | eclipse4diac::rtevents           |

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

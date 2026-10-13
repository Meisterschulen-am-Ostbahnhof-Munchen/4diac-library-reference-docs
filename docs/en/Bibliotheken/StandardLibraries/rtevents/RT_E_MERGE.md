# RT_E_MERGE

* * * * * * * * * *

## Introduction

Real-time version of the E_MERGE block. Merges multiple events.
![RT_E_MERGE](RT_E_MERGE.svg)

## Interface

### Event inputs

| Event | Associated variables | Description |
| :---- | :------------------- | :---------- |
| INIT | QI, Tmin, Deadline, WCET | Initialization |
| EI1 | - | First input event |
| EI2 | - | Second input event |

### Event outputs

| Event | Associated variables | Description |
| :---- | :------------------- | :---------- |
| INITO | QO | Confirmation of the initialization |
| EO | - | Output event |

### Input variables

| Name | Type | Description |
| :--- | :--- | :---------- |
| QI | BOOL | Initialization input |
| Tmin | TIME | Timing parameter of the real-time variant (not further commented in the source) |
| Deadline | TIME | Timing parameter of the real-time variant (not further commented in the source) |
| WCET | TIME | Timing parameter of the real-time variant (not further commented in the source) |

### Output variables

| Name | Type | Description |
| :--- | :--- | :---------- |
| QO | BOOL | Initialization output (with INITO) |

## Metadata

| Attribute     | Value                            |
| :------------ | :------------------------------- |
| License       | EPL-2.0                          |
| Version       | 3.0 (2025-04-14, Patrick Aigner) |
| 4diac Package | eclipse4diac::rtevents           |

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

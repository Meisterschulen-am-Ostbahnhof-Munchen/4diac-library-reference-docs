# RT_E_SPLIT

* * * * * * * * * *

## Introduction

Real-time version of the E_SPLIT block. Splits one event into multiple events.
![RT_E_SPLIT](RT_E_SPLIT.svg)

## Interface

| Port         | Type  | Description                                                                    |
| :----------- | :---- | :----------------------------------------------------------------------------- |
| INIT         | Event | Initialization (with QI, Tmin, Deadline_EO1, WCET_EO1, Deadline_EO2, WCET_EO2) |
| EI           | Event | Input event                                                                    |
| INITO        | Event | Initialization done (with QO)                                                  |
| EO1          | Event | First output event                                                             |
| EO2          | Event | Second output event                                                            |
| QI           | BOOL  | Input (with INIT)                                                              |
| Tmin         | TIME  | Real-time parameter (with INIT)                                                |
| Deadline_EO1 | TIME  | Real-time parameter for EO1 (with INIT)                                        |
| WCET_EO1     | TIME  | Real-time parameter for EO1 (with INIT)                                        |
| Deadline_EO2 | TIME  | Real-time parameter for EO2 (with INIT)                                        |
| WCET_EO2     | TIME  | Real-time parameter for EO2 (with INIT)                                        |
| QO           | BOOL  | Output (with INITO)                                                            |

## Metadata

| Attribute     | Value                            |
| :------------ | :------------------------------- |
| License       | EPL-2.0                          |
| Version       | 3.0 (2025-04-14, Patrick Aigner) |
| 4diac Package | eclipse4diac::rtevents           |

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

]

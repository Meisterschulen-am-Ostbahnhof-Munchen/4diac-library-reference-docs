# RT_E_DEMUX

* * * * * * * * * *

## Introduction

Real-time version of the E_DEMUX block. Forwards an input event to one of several outputs.
![RT_E_DEMUX](RT_E_DEMUX.svg)

## Interface

### Event inputs

| Event | Associated variables | Description |
| :---- | :------------------- | :---------- |
| INIT | QI, Tmin, Deadline0..Deadline3, WCET0..WCET3 | Initialization |
| EI | K | Event to demultiplex |

### Event outputs

| Event | Associated variables | Description |
| :---- | :------------------- | :---------- |
| INITO | QO | Confirmation of the initialization |
| EO0..EO3 | - | Output events (the number of outputs is implementation dependent) |

### Input variables

| Name | Type | Description |
| :--- | :--- | :---------- |
| QI | BOOL | Initialization input |
| K | UINT | Event index, the maximum is implementation dependent |
| Tmin | TIME | Timing parameter of the real-time variant (not further commented in the source) |
| Deadline0..Deadline3 | TIME | Timing parameter of the real-time variant (not further commented in the source), per output EO0..EO3 |
| WCET0..WCET3 | TIME | Timing parameter of the real-time variant (not further commented in the source), per output EO0..EO3 |

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

--

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

# AIS2

![AIS2](AIS2.svg)

bidirectional Adapter Interface for 1 Event and 1 STRING

## Interface

### Event Inputs

| Name | Comment                 | With |
| :--- | :---------------------- | :--- |
| EI1  | Request (or Indication) | DI1  |

### Event Outputs

| Name | Comment                 | With |
| :--- | :---------------------- | :--- |
| EO1  | Indication (or Request) | DO1  |

### Input Vars

| Name | Type | Comment                           |
| :--- | :--- | :-------------------------------- |
| DI1  | STRING | Request (or Indication) to Socket |

### Output Vars

| Name | Type | Comment                                |
| :--- | :--- | :------------------------------------- |
| DO1  | STRING | Indication (or Request) Data from Plug |

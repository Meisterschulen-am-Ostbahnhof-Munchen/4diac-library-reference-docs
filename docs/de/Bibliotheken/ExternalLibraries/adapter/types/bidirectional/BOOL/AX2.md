# AX2

**Plug (Adapter-Ausgang)**

![AX2_plug](AX2_plug.svg)

**Socket (Adapter-Eingang)**

![AX2_socket](AX2_socket.svg)

bidirectional Adapter Interface for 1 Event and 1 Bool

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
| DI1  | BOOL | Request (or Indication) to Socket |

### Output Vars

| Name | Type | Comment                                |
| :--- | :--- | :------------------------------------- |
| DO1  | BOOL | Indication (or Request) Data from Plug |

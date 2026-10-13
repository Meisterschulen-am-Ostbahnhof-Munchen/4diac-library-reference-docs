# AD2

**Plug (Adapter-Ausgang)**

![AD2_plug](AD2_plug.svg)

**Socket (Adapter-Eingang)**

![AD2_socket](AD2_socket.svg)

bidirectional Adapter Interface for 1 Event and 1 Dword

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
| DI1  | DWORD | Request (or Indication) to Socket |

### Output Vars

| Name | Type | Comment                                |
| :--- | :--- | :------------------------------------- |
| DO1  | DWORD | Indication (or Request) Data from Plug |

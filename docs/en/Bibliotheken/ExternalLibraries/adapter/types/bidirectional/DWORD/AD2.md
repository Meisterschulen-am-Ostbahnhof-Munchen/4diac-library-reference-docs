# AD2

**Plug (adapter output)**

![AD2_plug](AD2_plug.svg)

**Socket (adapter input)**

![AD2_socket](AD2_socket.svg)

bidirectional adapter interface for 1 event and 1 dword

## Interface

### Event inputs

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

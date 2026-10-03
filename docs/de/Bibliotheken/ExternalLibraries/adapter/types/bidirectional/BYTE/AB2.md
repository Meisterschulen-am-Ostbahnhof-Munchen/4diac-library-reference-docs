# AB2

**Plug (Adapter-Ausgang)**

![AB2_plug](AB2_plug.svg)

**Socket (Adapter-Eingang)**

![AB2_socket](AB2_socket.svg)

bidirectional Adapter Interface for 1 Event and 1 Byte

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
| DI1  | BYTE | Request (or Indication) to Socket |

### Output Vars

| Name | Type | Comment                                |
| :--- | :--- | :------------------------------------- |
| DO1  | BYTE | Indication (or Request) Data from Plug |

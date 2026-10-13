# AS2

![AS2](AS2.svg)

**Plug (adapter output)**

![AS2_plug](./AS2_plug.svg)

**Socket (adapter input)**

![AS2_socket](./AS2_socket.svg)

bidirectional Adapter Interface for 1 Event and 1 SINT

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
| DI1  | SINT | Request (or Indication) to Socket |

### Output Vars

| Name | Type | Comment                                |
| :--- | :--- | :------------------------------------- |
| DO1  | SINT | Indication (or Request) Data from Plug |

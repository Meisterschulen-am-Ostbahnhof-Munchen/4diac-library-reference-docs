# AE_AX

**Plug (Adapter-Ausgang)**

![AE_AX_plug](AE_AX_plug.svg)

**Socket (Adapter-Eingang)**

![AE_AX_socket](AE_AX_socket.svg)

bidirectional Adapter Interface for 1 Event (forward) and 1 Bool (backward, AX-style)

## Interface

### Event Inputs

| Name | Comment                 | With |
| :--- | :---------------------- | :--- |
| EI1  | Indication (or Request) | DI1  |

### Event Outputs

| Name | Comment                 | With |
| :--- | :---------------------- | :--- |
| E1   | Request (or Indication) |      |

### Input Vars

| Name | Type | Comment                              |
| :--- | :--- | :----------------------------------- |
| DI1  | BOOL | Indication (or Request) Data to Plug |

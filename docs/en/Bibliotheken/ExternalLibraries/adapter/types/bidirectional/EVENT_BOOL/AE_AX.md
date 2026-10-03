# AE_AX

**Plug (adapter output)**

![AE_AX_plug](AE_AX_plug.svg)

**Socket (adapter input)**

![AE_AX_socket](AE_AX_socket.svg)

bidirectional adapter interface for 1 event (forward) and 1 bool (backward, AX-style)

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

# ASR_AX

**Plug (adapter output)**

![ASR_AX_plug](ASR_AX_plug.svg)

**Socket (adapter input)**

![ASR_AX_socket](ASR_AX_socket.svg)

bidirectional adapter interface for 2 events (forward, Set/Reset) and 1 bool (backward, AX-style)

## Interface

### Event Inputs

| Name | Comment                 | With |
| :--- | :---------------------- | :--- |
| EI1  | Indication (or Request) | DI1  |

### Event Outputs

| Name  | Comment            | With |
| :---- | :----------------- | :--- |
| SET   | Set / Switch on    |      |
| RESET | Reset / Switch off |      |

### Input Vars

| Name | Type | Comment                              |
| :--- | :--- | :----------------------------------- |
| DI1  | BOOL | Indication (or Request) Data to Plug |

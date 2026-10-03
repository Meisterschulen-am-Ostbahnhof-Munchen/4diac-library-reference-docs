# ASRT_AX

**Plug (Adapter-Ausgang)**

![ASRT_AX_plug](ASRT_AX_plug.svg)

**Socket (Adapter-Eingang)**

![ASRT_AX_socket](ASRT_AX_socket.svg)

bidirectional Adapter Interface for 3 Events (forward, Set/Reset/Toggle) and 1 Bool (backward, AX-style)

## Interface

### Event Inputs

| Name | Comment                 | With |
| :--- | :---------------------- | :--- |
| EI1  | Indication (or Request) | DI1  |

### Event Outputs

| Name   | Comment                | With |
| :----- | :--------------------- | :--- |
| SET    | Set / Switch on        |      |
| RESET  | Reset / Switch off     |      |
| TOGGLE | Toggle / Switch output |      |

### Input Vars

| Name | Type | Comment                              |
| :--- | :--- | :----------------------------------- |
| DI1  | BOOL | Indication (or Request) Data to Plug |

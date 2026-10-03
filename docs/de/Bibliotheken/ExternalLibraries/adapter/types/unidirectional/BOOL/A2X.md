# A2X (BOOL)

**Plug (Adapter-Ausgang)**

![A2X_plug](A2X_plug.svg)

**Socket (Adapter-Eingang)**

![A2X_socket](A2X_socket.svg)

unidirectional Adapter Interface for 2 Events and 2 Bools

## Interface

### Events

| Name   | Comment | With |
| :----- | :------ | :--- |
| E_UP   | UP      | UP   |
| E_DOWN | DOWN    | DOWN |

### Data

| Name | Type | Comment                                        |
| :--- | :--- | :--------------------------------------------- |
| UP   | BOOL | TRUE = forward, up, right, clockwise           |
| DOWN | BOOL | TRUE = backward, down, left, counter-clockwise |

# A2X (BOOL)

**Plug (adapter output)**

![A2X_plug](A2X_plug.svg)

**Socket (adapter input)**

![A2X_socket](A2X_socket.svg)

Unidirectional adapter interface for 2 events and 2 bools

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

# FLOOR_AR

Adapter wrapper around `OSCAT::Basic::POUs::Mathematical::FLOOR`.  
Encapsulates rounding down to the largest integer (`INT`) behind a pure adapter boundary. An internal `E_D_FF_ANY` ensures `AI_OUT.E1` fires only on value changes.

## Interface

### Sockets

| Name  | Type | Comment |
| :---- | :--- | :------ |
| AR_IN | AR   | Input signal (REAL) |

### Plugs

| Name   | Type | Comment |
| :----- | :--- | :------ |
| AI_OUT | AI   | Largest integer <= X (INT) |

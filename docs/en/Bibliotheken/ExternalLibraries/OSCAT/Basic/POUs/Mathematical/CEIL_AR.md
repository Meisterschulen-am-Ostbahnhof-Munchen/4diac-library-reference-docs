# CEIL_AR

Adapter wrapper around `OSCAT::Basic::POUs::Mathematical::CEIL`.  
Encapsulates rounding up to the smallest integer (`INT`) behind a pure adapter boundary. An internal `E_D_FF_ANY` change filter ensures `AI_OUT.E1` only triggers when the integer output changes.

## Interface

### Sockets

| Name  | Type | Comment |
| :---- | :--- | :------ |
| AR_IN | AR   | Input signal (REAL) |

### Plugs

| Name   | Type | Comment |
| :----- | :--- | :------ |
| AI_OUT | AI   | Smallest integer >= X (INT) |

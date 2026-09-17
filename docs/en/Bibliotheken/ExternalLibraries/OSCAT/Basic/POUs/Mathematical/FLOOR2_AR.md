# FLOOR2_AR

Adapter wrapper around `OSCAT::Basic::POUs::Mathematical::FLOOR2`.  
Encapsulates rounding down to the largest 32-bit integer (`DINT`) behind a pure adapter boundary. An internal `E_D_FF_ANY` ensures `ADI_OUT.E1` fires only on value changes.

## Interface

### Sockets

| Name  | Type | Comment |
| :---- | :--- | :------ |
| AR_IN | AR   | Input signal (REAL) |

### Plugs

| Name    | Type | Comment |
| :------ | :--- | :------ |
| ADI_OUT | ADI  | Largest 32-bit integer <= X (DINT) |

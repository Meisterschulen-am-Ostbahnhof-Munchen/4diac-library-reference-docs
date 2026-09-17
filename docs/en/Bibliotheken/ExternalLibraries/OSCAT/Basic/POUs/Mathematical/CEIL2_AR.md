# CEIL2_AR

Adapter wrapper around `OSCAT::Basic::POUs::Mathematical::CEIL2`.  
Encapsulates rounding up to the smallest 32-bit integer (`DINT`) behind a pure adapter boundary. An internal `E_D_FF_ANY` ensures `ADI_OUT.E1` only fires when the value actually changes.

## Interface

### Sockets

| Name  | Type | Comment |
| :---- | :--- | :------ |
| AR_IN | AR   | Input signal (REAL) |

### Plugs

| Name    | Type | Comment |
| :------ | :--- | :------ |
| ADI_OUT | ADI  | Smallest 32-bit integer >= X (DINT) |

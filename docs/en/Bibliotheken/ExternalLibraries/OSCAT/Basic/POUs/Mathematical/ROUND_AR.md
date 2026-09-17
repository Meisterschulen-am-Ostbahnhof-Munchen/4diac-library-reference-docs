# ROUND_AR

Adapter wrapper around `OSCAT::Basic::POUs::Mathematical::ROUND`.  
Rounds the input signal received via `AR_IN` to `N` decimal places. Parameter `N` is provided as an `InputVar` ($0 \dots 8$). An internal `E_D_FF_ANY` ensures `AR_OUT.E1` fires only when the rounded value changes.

## Interface

### Sockets

| Name  | Type | Comment |
| :---- | :--- | :------ |
| AR_IN | AR   | Input signal (REAL) |

### Input Vars

| Name | Type | Comment |
| :--- | :--- | :------ |
| N    | INT  | Number of decimal places (0..8) |

### Plugs

| Name   | Type | Comment |
| :----- | :--- | :------ |
| AR_OUT | AR   | Rounded output signal (REAL) |

# RND_AR

Adapter wrapper around `OSCAT::Basic::POUs::Mathematical::RND`.  
Rounds the input signal received via `AR_IN` to `N` significant digits. Parameter `N` is provided as an `InputVar` ($1 \dots 8$). An internal `E_D_FF_ANY` filters out redundant event triggers at `AR_OUT.E1`.

## Interface

### Sockets

| Name  | Type | Comment |
| :---- | :--- | :------ |
| AR_IN | AR   | Input signal (REAL) |

### Input Vars

| Name | Type | Comment |
| :--- | :--- | :------ |
| N    | INT  | Number of significant digits (1..8) |

### Plugs

| Name   | Type | Comment |
| :----- | :--- | :------ |
| AR_OUT | AR   | Rounded output signal (REAL) |

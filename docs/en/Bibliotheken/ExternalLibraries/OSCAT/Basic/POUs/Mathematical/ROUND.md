# ROUND

Rounds a floating point number (`in`) to `N` digits after the decimal point.  
The parameter `N` is constrained to the range $0 \dots 8$ to prevent DINT overflow during scaling.

## Interface

### Event Inputs

| Name | Comment | With |
| :--- | :------ | :--- |
| REQ  |         | in, N |

### Event Outputs

| Name | Comment | With |
| :--- | :------ | :--- |
| CNF  |         |      |

### Input Vars

| Name | Type | Comment |
| :--- | :--- | :------ |
| in   | REAL | Input value |
| N    | INT  | Number of decimal places (0..8) |

### Output Vars

| Name | Type | Comment |
| :--- | :--- | :------ |
|      | REAL | Rounded value |

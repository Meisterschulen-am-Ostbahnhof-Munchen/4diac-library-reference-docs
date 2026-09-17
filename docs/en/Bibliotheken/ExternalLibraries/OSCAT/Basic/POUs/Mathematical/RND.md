# RND

Rounds a floating point number (`REAL`) to `N` significant digits (not decimal places).  
The parameter `N` is constrained to the range $1 \dots 8$ to prevent DINT overflow during scaling.

## Interface

### Event Inputs

| Name | Comment | With |
| :--- | :------ | :--- |
| REQ  |         | X, N |

### Event Outputs

| Name | Comment | With |
| :--- | :------ | :--- |
| CNF  |         |      |

### Input Vars

| Name | Type | Comment |
| :--- | :--- | :------ |
| X    | REAL | Input value |
| N    | INT  | Number of significant digits (1..8) |

### Output Vars

| Name | Type | Comment |
| :--- | :--- | :------ |
|      | REAL | Rounded value to N significant digits |

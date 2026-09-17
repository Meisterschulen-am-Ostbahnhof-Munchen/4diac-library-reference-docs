# RND

Rundet eine Gleitkommazahl (`REAL`) auf `N` signifikante Stellen (nicht Nachkommastellen).  
Der Parameter `N` ist auf den Bereich $1 \dots 8$ begrenzt, um DINT-Überläufe bei der Skalierung zu verhindern.

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
| X    | REAL | Eingangswert |
| N    | INT  | Anzahl signifikanter Stellen (1..8) |

### Output Vars

| Name | Type | Comment |
| :--- | :--- | :------ |
|      | REAL | Gerundeter Wert auf N signifikante Stellen |

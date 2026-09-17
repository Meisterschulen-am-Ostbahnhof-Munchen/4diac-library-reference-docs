# ROUND

Rundet eine Gleitkommazahl (`in`) auf `N` Nachkommastellen.  
Der Parameter `N` ist auf den Bereich $0 \dots 8$ begrenzt, um DINT-Überläufe bei der Skalierung zu verhindern.

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
| in   | REAL | Eingangswert |
| N    | INT  | Anzahl Nachkommastellen (0..8) |

### Output Vars

| Name | Type | Comment |
| :--- | :--- | :------ |
|      | REAL | Gerundeter Wert |

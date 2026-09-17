# ROUND_AR

Adapter-Wrapper um `OSCAT::Basic::POUs::Mathematical::ROUND`.  
Rundet das über `AR_IN` empfangene Signal auf `N` Nachkommastellen. Der Parameter `N` wird als `InputVar` angegeben ($0 \dots 8$). Ein internes `E_D_FF_ANY` stellt sicher, dass `AR_OUT.E1` nur feuert, wenn sich das gerundete Ergebnis geändert hat.

## Interface

### Sockets

| Name  | Type | Comment |
| :---- | :--- | :------ |
| AR_IN | AR   | Eingangssignal (REAL) |

### Input Vars

| Name | Type | Comment |
| :--- | :--- | :------ |
| N    | INT  | Anzahl Nachkommastellen (0..8) |

### Plugs

| Name   | Type | Comment |
| :----- | :--- | :------ |
| AR_OUT | AR   | Gerundetes Ausgangssignal (REAL) |

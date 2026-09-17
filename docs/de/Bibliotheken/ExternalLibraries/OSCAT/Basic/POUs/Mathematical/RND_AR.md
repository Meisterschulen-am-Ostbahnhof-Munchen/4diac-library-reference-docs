# RND_AR

Adapter-Wrapper um `OSCAT::Basic::POUs::Mathematical::RND`.  
Rundet das über `AR_IN` empfangene Signal auf `N` signifikante Stellen. Der Parameter `N` wird als `InputVar` angegeben ($1 \dots 8$). Ein internes `E_D_FF_ANY` schützt vor unnötigen Event-Auslösungen an `AR_OUT.E1`.

## Interface

### Sockets

| Name  | Type | Comment |
| :---- | :--- | :------ |
| AR_IN | AR   | Eingangssignal (REAL) |

### Input Vars

| Name | Type | Comment |
| :--- | :--- | :------ |
| N    | INT  | Anzahl signifikanter Stellen (1..8) |

### Plugs

| Name   | Type | Comment |
| :----- | :--- | :------ |
| AR_OUT | AR   | Gerundetes Ausgangssignal (REAL) |

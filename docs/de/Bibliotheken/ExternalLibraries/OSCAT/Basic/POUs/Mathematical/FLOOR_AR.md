# FLOOR_AR

Adapter-Wrapper um `OSCAT::Basic::POUs::Mathematical::FLOOR`.  
Kapselt die Abrundung auf die nächstkleinere ganze Zahl (`INT`) hinter einer rein adapterbasierten Schnittstelle. Ein internes `E_D_FF_ANY` sorgt dafür, dass `AI_OUT.E1` nur bei einer Wertänderung feuert.

## Interface

### Sockets

| Name  | Type | Comment |
| :---- | :--- | :------ |
| AR_IN | AR   | Eingangssignal (REAL) |

### Plugs

| Name   | Type | Comment |
| :----- | :--- | :------ |
| AI_OUT | AI   | Größte ganze Zahl <= X (INT) |

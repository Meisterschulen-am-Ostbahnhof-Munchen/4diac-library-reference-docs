# FLOOR2_AR

Adapter-Wrapper um `OSCAT::Basic::POUs::Mathematical::FLOOR2`.  
Kapselt die Abrundung auf die nächstkleinere 32-Bit Ganzzahl (`DINT`) hinter einer rein adapterbasierten Schnittstelle. Ein internes `E_D_FF_ANY` stellt sicher, dass `ADI_OUT.E1` nur bei einer tatsächlichen Wertänderung feuert.

## Interface

### Sockets

| Name  | Type | Comment |
| :---- | :--- | :------ |
| AR_IN | AR   | Eingangssignal (REAL) |

### Plugs

| Name    | Type | Comment |
| :------ | :--- | :------ |
| ADI_OUT | ADI  | Größte Ganzzahl <= X (DINT) |

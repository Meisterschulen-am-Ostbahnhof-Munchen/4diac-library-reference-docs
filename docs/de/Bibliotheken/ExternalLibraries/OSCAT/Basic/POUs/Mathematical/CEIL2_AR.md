# CEIL2_AR

Adapter-Wrapper um `OSCAT::Basic::POUs::Mathematical::CEIL2`.  
Kapselt die Aufrundung auf die nächstgrößere 32-Bit Ganzzahl (`DINT`) hinter einer rein adapterbasierten Schnittstelle. Ein internes `E_D_FF_ANY` stellt sicher, dass `ADI_OUT.E1` nur bei einer tatsächlichen Wertänderung feuert.

## Interface

### Sockets

| Name  | Type | Comment |
| :---- | :--- | :------ |
| AR_IN | AR   | Eingangssignal (REAL) |

### Plugs

| Name    | Type | Comment |
| :------ | :--- | :------ |
| ADI_OUT | ADI  | Kleinste Ganzzahl >= X (DINT) |

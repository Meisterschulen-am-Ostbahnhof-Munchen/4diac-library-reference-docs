# CEIL_AR

Adapter-Wrapper um `OSCAT::Basic::POUs::Mathematical::CEIL`.  
Kapselt die Aufrundung auf die nächstgrößere ganze Zahl (`INT`) hinter einer rein adapterbasierten Schnittstelle. Ein internes `E_D_FF_ANY` (Change-Filter) stellt sicher, dass `AI_OUT.E1` nur auslöst, wenn sich der gerundete Integer-Wert geändert hat.

## Interface

### Sockets

| Name  | Type | Comment |
| :---- | :--- | :------ |
| AR_IN | AR   | Eingangssignal (REAL) |

### Plugs

| Name   | Type | Comment |
| :----- | :--- | :------ |
| AI_OUT | AI   | Kleinste ganze Zahl >= X (INT) |

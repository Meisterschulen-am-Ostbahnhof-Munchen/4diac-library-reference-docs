# RT_E_REND

* * * * * * * * * *

## Einleitung

Echtzeit-Variante des E_REND Bausteins (Rendezvous). Wartet auf zwei Ereignisse.

![RT_E_REND](RT_E_REND.svg)

## Schnittstelle

| Port     | Typ   | Beschreibung                                    |
| :------- | :---- | :---------------------------------------------- |
| INIT     | Event | Initialisierung (mit QI, Tmin, Deadline, WCET)  |
| EI1      | Event | Erster Ereigniseingang                          |
| EI2      | Event | Zweiter Ereigniseingang                         |
| R        | Event | Reset-Ereignis                                  |
| INITO    | Event | Initialisierung abgeschlossen (mit QO)          |
| EO       | Event | Rendezvous-Ausgangsereignis                     |
| QI       | BOOL  | Eingang (mit INIT)                              |
| Tmin     | TIME  | Echtzeit-Parameter (mit INIT)                   |
| Deadline | TIME  | Echtzeit-Parameter (mit INIT)                   |
| WCET     | TIME  | Echtzeit-Parameter (mit INIT)                   |
| QO       | BOOL  | Ausgang (mit INITO)                             |

## Metadaten

| Attribut    | Wert                             |
| :---------- | :------------------------------- |
| Lizenz      | EPL-2.0                          |
| Version     | 3.0 (2025-04-14, Patrick Aigner) |
| 4diac-Paket | eclipse4diac::rtevents           |

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

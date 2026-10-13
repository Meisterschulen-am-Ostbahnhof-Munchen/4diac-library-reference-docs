# RT_E_F_TRIG

* * * * * * * * * *

## Einleitung

Echtzeit-Variante des E_F_TRIG Bausteins (Fallende Flanke).

![RT_E_F_TRIG](RT_E_F_TRIG.svg)

## Schnittstelle

| Port     | Typ   | Beschreibung                                    |
| :------- | :---- | :---------------------------------------------- |
| INIT     | Event | Initialisierung (mit QI, Tmin, Deadline, WCET)  |
| EI       | Event | Ereigniseingang (mit QI)                        |
| INITO    | Event | Initialisierung abgeschlossen (mit QO)          |
| EO       | Event | Ereignisausgang                                 |
| QI       | BOOL  | Eingangssignal                                  |
| Tmin     | TIME  | Echtzeit-Parameter (mit INIT)                   |
| Deadline | TIME  | Echtzeit-Parameter (mit INIT)                   |
| WCET     | TIME  | Echtzeit-Parameter (mit INIT)                   |
| QO       | BOOL  | Ausgangssignal (mit INITO)                      |

## Metadaten

| Attribut    | Wert                             |
| :---------- | :------------------------------- |
| Lizenz      | EPL-2.0                          |
| Version     | 3.0 (2025-04-14, Patrick Aigner) |
| 4diac-Paket | eclipse4diac::rtevents           |

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

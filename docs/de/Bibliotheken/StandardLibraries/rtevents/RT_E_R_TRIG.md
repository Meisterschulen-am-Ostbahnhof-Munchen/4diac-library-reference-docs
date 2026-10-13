# RT_E_R_TRIG

* * * * * * * * * *

## Einleitung

Echtzeit-Variante des E_R_TRIG Bausteins (Steigende Flanke).

![RT_E_R_TRIG](RT_E_R_TRIG.svg)

## Schnittstelle

### Ereigniseingänge

| Ereignis | Zugehörige Variablen | Beschreibung |
| :------- | :------------------- | :----------- |
| INIT | QI, Tmin, Deadline, WCET | Initialisierung |
| EI | QI | Ereigniseingang |

### Ereignisausgänge

| Ereignis | Zugehörige Variablen | Beschreibung |
| :------- | :------------------- | :----------- |
| INITO | QO | Bestätigung der Initialisierung |
| EO | - | Ereignisausgang |

### Eingangsvariablen

| Name | Typ | Beschreibung |
| :--- | :-- | :----------- |
| QI | BOOL | Eingang (bei INIT: Initialisierung; bei EI: zu bewertender Wert) |
| Tmin | TIME | Zeitparameter der Echtzeit-Variante (im Quelltext nicht weiter kommentiert) |
| Deadline | TIME | Zeitparameter der Echtzeit-Variante (im Quelltext nicht weiter kommentiert) |
| WCET | TIME | Zeitparameter der Echtzeit-Variante (im Quelltext nicht weiter kommentiert) |

### Ausgangsvariablen

| Name | Typ | Beschreibung |
| :--- | :-- | :----------- |
| QO | BOOL | Ausgang der Initialisierung (zu INITO) |

## Metadaten

| Attribut    | Wert                             |
| :---------- | :------------------------------- |
| Lizenz      | EPL-2.0                          |
| Version     | 3.0 (2025-04-14, Patrick Aigner) |
| 4diac-Paket | eclipse4diac::rtevents           |

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

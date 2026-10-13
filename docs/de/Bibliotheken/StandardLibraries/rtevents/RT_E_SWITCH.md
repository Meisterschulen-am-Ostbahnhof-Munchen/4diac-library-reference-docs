# RT_E_SWITCH

* * * * * * * * * *

## Einleitung

Echtzeit-Variante des E_SWITCH Bausteins. Leitet das Eingangsereignis EI abhängig von G an einen der beiden Ausgänge EO1/EO2 weiter.

![RT_E_SWITCH](RT_E_SWITCH.svg)

## Schnittstelle

### Ereigniseingänge

| Ereignis | Zugehörige Variablen | Beschreibung |
| :------- | :------------------- | :----------- |
| INIT | QI, Tmin, Deadline_EO1, WCET_EO1, Deadline_EO2, WCET_EO2 | Initialisierung |
| EI | G | Eingangsereignis |

### Ereignisausgänge

| Ereignis | Zugehörige Variablen | Beschreibung |
| :------- | :------------------- | :----------- |
| INITO | QO | Bestätigung der Initialisierung |
| EO1 | - | Erstes Ausgangsereignis |
| EO2 | - | Zweites Ausgangsereignis |

### Eingangsvariablen

| Name | Typ | Beschreibung |
| :--- | :-- | :----------- |
| QI | BOOL | Eingang der Initialisierung |
| G | BOOL | Schaltbedingung, mit EI verknüpft |
| Tmin | TIME | Zeitparameter der Echtzeit-Variante (im Quelltext nicht weiter kommentiert) |
| Deadline_EO1, Deadline_EO2 | TIME | Zeitparameter der Echtzeit-Variante (im Quelltext nicht weiter kommentiert), je Ausgang |
| WCET_EO1, WCET_EO2 | TIME | Zeitparameter der Echtzeit-Variante (im Quelltext nicht weiter kommentiert), je Ausgang |

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

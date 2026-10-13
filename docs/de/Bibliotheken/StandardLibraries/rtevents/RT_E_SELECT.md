# RT_E_SELECT

* * * * * * * * * *

## Einleitung

Echtzeit-Variante des E_SELECT Bausteins. Wählt eines von zwei Eingangsereignissen basierend auf einem Selektor aus.

![RT_E_SELECT](RT_E_SELECT.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT**: Initialisierung, trägt `QI`, `Tmin`, `Deadline` und `WCET`.
- **EI0**: Eingangsereignis, wird bei `G` = 0 ausgewählt; trägt `G`.
- **EI1**: Eingangsereignis, wird bei `G` = 1 ausgewählt; trägt `G`.

### **Ereignis-Ausgänge**

- **INITO**: Bestätigt die Initialisierung, trägt `QO`.
- **EO**: Ausgangsereignis; trägt keine Daten.

### **Daten-Eingänge**

- **QI** (`BOOL`): Qualifier für die Initialisierung.
- **G** (`BOOL`): Selektor: `EI0` bei `G` = 0, `EI1` bei `G` = 1.
- **Tmin** (`TIME`): Echtzeitparameter (minimaler Abstand).
- **Deadline** (`TIME`): Echtzeitparameter (Deadline).
- **WCET** (`TIME`): Echtzeitparameter (maximale Ausführungszeit).

### **Daten-Ausgänge**

- **QO** (`BOOL`): Ausgangsqualifier der Initialisierung.

## Metadaten

| Attribut    | Wert                             |
| :---------- | :------------------------------- |
| Lizenz      | EPL-2.0                          |
| Version     | 3.0 (2025-04-14, Patrick Aigner) |
| 4diac-Paket | eclipse4diac::rtevents           |

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

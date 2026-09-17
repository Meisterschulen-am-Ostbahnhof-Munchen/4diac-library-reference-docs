# TIME_TO_REAL

![TIME_TO_REAL](TIME_TO_REAL.svg)

* * * * * * * * * *

## Einleitung

`TIME_TO_REAL` ist eine projekteigene Hilfsfunktion (`FunctionType`), die eine Zeitdauer vom Typ `TIME` in einen Fließkommawert (`REAL`) umrechnet. Das Ergebnis entspricht der Zeitdauer in **Sekunden**.

Diese Funktion wird innerhalb von OSCAT-Regelungs- und Filterbausteinen (wie `FT_PT1`) verwendet, um Zeitangaben aus der SPS-Umgebung für mathematische Differential- und Integrationsberechnungen aufzubereiten.

## Schnittstellenstruktur

### **Eingänge**

| Name | Typ | Beschreibung |
| :--- | :--- | :----------- |
| `IN` | `TIME` | Eingangs-Zeitdauer |

### **Ausgänge**

| Name | Typ | Beschreibung |
| :--- | :--- | :----------- |
| *(Return)* | `REAL` | Berechnete Zeitdauer in Sekunden |

## Funktionsweise

Die Funktion rechnet den internen Millisekunden-Wert des `TIME`-Eingangs in Sekunden um:

$$\text{Return} = \text{TIME\_TO\_UDINT}(\text{IN}) \cdot 1.0 \times 10^{-3}$$

Beispiel:

- `IN = T#1s` $\rightarrow$ `Return = 1.0`
- `IN = T#500ms` $\rightarrow$ `Return = 0.5`
- `IN = T#2m30s` $\rightarrow$ `Return = 150.0`

## Technische Besonderheiten

- **Projektspezifische Erweiterung**: `TIME_TO_REAL` ist keine Standard-Funktion der OSCAT-Bibliothek, sondern ein lokaler Konvertierungshelfer für IEC 61499.
- **Skalierungsgenauigkeit**: Verwendet explizite Fließkommaskalierung ($1.0 \times 10^{-3}$), um Ganzzahl-Abrundungsfehler bei der Division zu vermeiden.

## Siehe auch

- [`FT_PT1`](FT_PT1.md) – Tiefpassfilter 1. Ordnung (verwendet `TIME_TO_REAL`).
- [`FT_PT2`](FT_PT2.md) – Tiefpassfilter 2. Ordnung.

# DualHysteresis_Thresholds_AR

![DualHysteresis_Thresholds_AR_network](./DualHysteresis_Thresholds_AR_network.svg)

* * * * * * * * * *

## Einleitung

`DualHysteresis_Thresholds_AR` berechnet aus dem Sollwert `MI` (Mittelwert), der Totzone `DEAD` und der Hysterese `HYST` die fünf Schwellwerte einer Zwei-Wege-Hysterese (z. B. Heben/Senken) ausschließlich über AR-Adapter. Zusätzlich werden `MI`, `DEAD` und `HYST` über die Durchreich-Plugs `MI_OUT`, `DEAD_OUT` und `HYST_OUT` weitergegeben.

![DualHysteresis_Thresholds_AR](DualHysteresis_Thresholds_AR.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Eingänge.

### **Ereignis-Ausgänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Ausgänge.

### **Daten-Eingänge**

Der Funktionsblock verfügt über keine direkten Daten-Eingänge.

### **Daten-Ausgänge**

Der Funktionsblock verfügt über keine direkten Daten-Ausgänge.

### **Adapter**

- **MI** (Socket, `AR`): Sollwert / Mittelwert (Center Point)
- **DEAD** (Socket, `AR`): Totzone um `MI` (Deadband)
- **HYST** (Socket, `AR`): Hysterese zusätzlich zur Totzone
- **MID_MINUS_DEAD_HYST** (Plug, `AR`): Schwelle 1 = `MI - (DEAD + HYST)` [Min / Heben Ein / Senken Start]
- **MID_MINUS_DEAD** (Plug, `AR`): Schwelle 2 = `MI - DEAD` [Senken Stopp]
- **MID_OUT** (Plug, `AR`): Schwelle 3 = `MI` (Mitte / Sollwert), z. B. für einen Schwellwert-Marker
- **MID_PLUS_DEAD** (Plug, `AR`): Schwelle 4 = `MI + DEAD` [Heben Stopp]
- **MID_PLUS_DEAD_HYST** (Plug, `AR`): Schwelle 5 = `MI + (DEAD + HYST)` [Max / Heben Start]
- **MI_OUT**, **DEAD_OUT**, **HYST_OUT** (Plugs, `AR`): Durchreich-Plugs für `MI`, `DEAD` und `HYST` (z. B. für `DualHysteresis_AR_AX`)

## Funktionsweise

Jeder Eingang wird zuerst durch einen Split-Baustein auf mehrere Verbraucher verteilt, weil ein Adapter nie ohne Split verzweigt werden soll:

- `MI` → `Split_MI` (`AR_SPLIT_4`): `OUT1` → `MID_OUT`, `OUT2` → `Add_MidDead.IN1`, `OUT3` → `Sub_MidDead.IN1`, `OUT4` → `MI_OUT`.
- `DEAD` → `Split_DEAD` (`AR_SPLIT_3`): `OUT1` → `Add_MidDead.IN2`, `OUT2` → `Sub_MidDead.IN2`, `OUT3` → `DEAD_OUT`.
- `HYST` → `Split_HYST` (`AR_SPLIT_3`): `OUT1` → `Add_MidDeadHyst.IN2`, `OUT2` → `Sub_MidDeadHyst.IN2`, `OUT3` → `HYST_OUT`.

Die Rechenkette besteht aus `AR_ADD_2` und `AR_SUB_2` (`adapter::iec61131::arithmetic`):

- `Add_MidDead` = `MI + DEAD` → `Split_AddResult` (`AR_SPLIT_2`): `OUT1` → `MID_PLUS_DEAD`, `OUT2` → `Add_MidDeadHyst.IN1`.
- `Sub_MidDead` = `MI - DEAD` → `Split_SubResult` (`AR_SPLIT_2`): `OUT1` → `MID_MINUS_DEAD`, `OUT2` → `Sub_MidDeadHyst.IN1`.
- `Add_MidDeadHyst` = `(MI + DEAD) + HYST` → `MID_PLUS_DEAD_HYST`.
- `Sub_MidDeadHyst` = `(MI - DEAD) - HYST` → `MID_MINUS_DEAD_HYST`.

## Technische Besonderheiten

- Reiner Rechen-/Verdrahtungsbaustein ohne eigenen Zustand; die Ausgänge folgen den Eingängen
- `MI_OUT` ist vom Schwellwert `MID_OUT` getrennt (Version 1.1: `Split_MI` von `AR_SPLIT_3` auf `AR_SPLIT_4` erweitert), damit jeder Plug eine eigene Quelle hat
- Die Beschriftungen in eckigen Klammern (Heben/Senken) sind die vorgesehene Verwendung der Schwellen in der Aufrufer-Logik

## Anwendungsszenarien

- Zwei-Wege-Regelung (Heben/Senken) mit Totzone und Hysterese, z. B. Füllband- oder Hangausgleich-Höhenregelung, einschließlich Schwellwert-Markern am VT

## Fazit

`DualHysteresis_Thresholds_AR` liefert aus `MI`, `DEAD` und `HYST` per AR-Adapter-Arithmetik die fünf Schwellwerte `MI ± DEAD` und `MI ± (DEAD + HYST)` sowie `MI` selbst und reicht die drei Eingangsgrößen unverändert weiter.

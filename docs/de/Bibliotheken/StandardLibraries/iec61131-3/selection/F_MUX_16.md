# F_MUX_16

![F_MUX_16](F_MUX_16.svg)

* * * * * * * * * *

## Einleitung

Der `F_MUX_16` ist ein Multiplexer nach IEC 61131-3 (Standard-Auswahlfunktion, generischer Baustein), der anhand des Selektors `K` einen von **16 Eingängen** auf den Ausgang `OUT` durchschaltet. Er ist die 16-Kanal-Variante von [`F_MUX_8`](F_MUX_8.md) und [`F_MUX_4`](F_MUX_4.md).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **REQ**: Serviceanforderung. Verbundene Daten: `IN1` bis `IN16`, `K`

### **Ereignis-Ausgänge**

- **CNF**: Bestätigung des angeforderten Dienstes. Verbundene Daten: `OUT`

### **Daten-Eingänge**

- **K** (`ANY_INT`): Wählt einen der Eingänge aus
- **IN1** bis **IN16** (`ANY`): Eingangswerte 1 bis 16

### **Daten-Ausgänge**

- **OUT** (`ANY`): Gewählter Eingangswert (siehe Tabelle)

### **Adapter**

Dieser Funktionsblock verwendet keine Adapter.

## Funktionsweise

Bei `REQ` wird der durch `K` gewählte Eingang an `OUT` ausgegeben, anschließend wird `CNF` ausgelöst. `K` zählt ab 0:

| K | OUT | K | OUT |
| --- | --- | --- | --- |
| 0 | `IN1` | 8 | `IN9` |
| 1 | `IN2` | 9 | `IN10` |
| 2 | `IN3` | 10 | `IN11` |
| 3 | `IN4` | 11 | `IN12` |
| 4 | `IN5` | 12 | `IN13` |
| 5 | `IN6` | 13 | `IN14` |
| 6 | `IN7` | 14 | `IN15` |
| 7 | `IN8` | 15 | `IN16` |

## Technische Besonderheiten

- Generischer Baustein (`GEN_F_MUX`, FORTE); die Datentypen der Eingänge sind `ANY` und müssen zu `OUT` passen.
- Gehört zum Paket `iec61131::selection`.

## Anwendungsszenarien

- Auswahl eines von bis zu 16 Werten, z. B. Konstanten, Sollwerte oder Objekt-IDs, über einen Index

## Fazit

`F_MUX_16` schaltet anhand des Index `K` (0 bis 15) einen von 16 Eingangswerten auf den Ausgang.

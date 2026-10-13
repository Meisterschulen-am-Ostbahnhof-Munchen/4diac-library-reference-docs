# AX_T_FF_SR_GATE

![AX_T_FF_SR_GATE_ecc](./AX_T_FF_SR_GATE_ecc.svg)

* * * * * * * * * *

## Einleitung

`AX_T_FF_SR_GATE` entspricht [`AX_T_FF_SR`](./AX_T_FF_SR.md), hat aber zusätzlich einen Freigabe-Socket `GATE` (`AX`) und einen eigenen Zustand `GATE_CLOSED`, ohne Verkettung (kein `CHAIN_IN`/`CHAIN_OUT`). Er eignet sich für Fälle, in denen nur die Gate-Sperre gebraucht wird, aber keine Kettenverzögerung; mit Verkettung siehe [`AX_T_FF_SR_SEQ_GATE`](./AX_T_FF_SR_SEQ_GATE.md).

![AX_T_FF_SR_GATE](AX_T_FF_SR_GATE.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **S**: Setzt den Ausgang `Q` – wirkungslos während `GATE_CLOSED`
- **R**: Setzt den Ausgang `Q` zurück – wirkungslos während `GATE_CLOSED` (ist dort ohnehin aus)
- **CLK**: Takt zum Umschalten (Toggle) des Ausgangs – wirkungslos während `GATE_CLOSED`

### **Ereignis-Ausgänge**

Keine direkten Ereignis-Ausgänge – das Ereignis wird über `Q.E1` ausgegeben.

### **Daten-Eingänge / Daten-Ausgänge**

Keine.

### **Adapter**

- **Q** (Plug, `adapter::types::unidirectional::AX`): Wert des Flipflops
- **GATE** (Socket, `adapter::types::unidirectional::AX`): Freigabe, muss `TRUE` sein. Fällt `GATE` auf `FALSE`, geht der Baustein sofort in `GATE_CLOSED` (Zwangs-Reset und Sperre); verlassen wird dieser Zustand nur über `GATE.E1[GATE.D1]`

## Funktionsweise

Zustände und Aktionen:

- **START**: Initialzustand
- **SET**: Algorithmus `SET` (`Q.D1 := TRUE`), Ausgabe `Q.E1`
- **RESET**: Algorithmus `RESET` (`Q.D1 := FALSE`), Ausgabe `Q.E1`
- **GATE_CLOSED**: gesperrt (`GATE.D1 = FALSE`), `Q` zwangsweise `FALSE` (Algorithmus `RESET`, Ausgabe `Q.E1`); `S`/`R`/`CLK` sind hier wirkungslos

Transitionen (in Deklarationsreihenfolge):

| Von | Nach | Bedingung |
| :--- | :--- | :--- |
| START | GATE_CLOSED | `[NOT GATE.D1]` |
| START | SET | `S[GATE.D1]` |
| START | SET | `CLK[GATE.D1]` |
| SET | GATE_CLOSED | `[NOT GATE.D1]` |
| SET | RESET | `R` |
| SET | RESET | `CLK` |
| RESET | GATE_CLOSED | `[NOT GATE.D1]` |
| RESET | SET | `S[GATE.D1]` |
| RESET | SET | `CLK[GATE.D1]` |
| GATE_CLOSED | RESET | `GATE.E1[GATE.D1]` |

## Technische Besonderheiten

- **Sperre zuerst**: `[NOT GATE.D1]` steht in jedem Zustand an erster Stelle der Deklarationsreihenfolge. Da `S`/`CLK` ohne Guard immer `TRUE` sind, würde sonst nach IEC 61499-1 Tabelle 1 Regel c) die Gate-Sperre nie zum Zug kommen (Bugfix v1.2). Ist das Gate offen, ist der Guard `FALSE` und wird übersprungen
- **Boot-Fenster**: Die ereignislose Transition `START` → `GATE_CLOSED` wird sofort beim Betreten von `START` ausgewertet und deckt den Zustand ab, bevor je ein `GATE.E1` gefeuert hat (v1.1)
- Aus `GATE_CLOSED` geht es nur nach `RESET` zurück, `Q` bleibt also `FALSE`, bis `S` oder `CLK` wirkt
- Kopie von `AX_T_FF_SR` mit `GATE`-Socket und Zustand `GATE_CLOSED`, identisches Muster wie `AX_T_FF_SR_SEQ_GATE`

## Anwendungsszenarien

- Toggle-/Set-Reset-Ausgang, der durch eine Freigabe zwangsweise gesperrt werden muss (z. B. Sicherheits- oder Betriebsfreigabe)

## Fazit

`AX_T_FF_SR_GATE` ist ein Toggle-/SR-Flipflop mit Freigabe: Ist `GATE` `FALSE`, wird `Q` zurückgesetzt und alle Eingänge bleiben bis zur Freigabe wirkungslos.

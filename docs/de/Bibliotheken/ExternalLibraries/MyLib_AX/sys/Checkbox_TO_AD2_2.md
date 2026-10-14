# Checkbox_TO_AD2_2

![Checkbox_TO_AD2_2_network](./Checkbox_TO_AD2_2_network.svg)

* * * * * * * * * *

## Einleitung

`Checkbox_TO_AD2_2` fasst zwei VT-Checkboxen zu einer Bitmaske zusammen und reicht sie bidirektional als AD2-Adapter (`K`) an einen Consumer weiter (z. B. `AOption_AD_DEMUX_2.K`). Checkbox 1 entspricht Bit 0, Checkbox 2 Bit 1. Die vom Consumer zurückgemeldete (tatsächlich aktive) Bitmaske wird wieder auf die beiden Checkboxen zurückgeschrieben.

![Checkbox_TO_AD2_2](Checkbox_TO_AD2_2.svg)

## Schnittstelle

### Ereignis-Eingänge

- `INIT` (`EInit`): Initialization Request.

### Ereignis-Ausgänge

- `INITO` (`EInit`): Initialization Confirm.

### Eingangsvariablen

- `QI` (BOOL, Default `TRUE`): Quality Input, TRUE aktiviert den Dienst.
- `u16ObjId_1` (UINT, Default `ID_NULL`): ObjectID Checkbox 1 (Bit 0).
- `u16ObjId_2` (UINT, Default `ID_NULL`): ObjectID Checkbox 2 (Bit 1).

### Ausgangsvariablen

- `QO` (BOOL): Quality Output, UND-Verknüpfung (`AND_QO`, `AND_BOOL_4`) der `QO`-Ausgänge der beiden `Checkbox_IDA_TO_AX`- und der beiden `AX_TO_Checkbox`-Instanzen.

### Adapter

- **Plug `K`** (`adapter::types::bidirectional::AD2`): Bitmaske zum Consumer und Echo zurück.

Die Initialisierung läuft als Kette durch die vier Unterbausteine (siehe unten).

## Funktionsweise

Hinrichtung (VT → Consumer):

1. Zwei Instanzen von [`Checkbox_IDA_TO_AX`](./Checkbox_IDA_TO_AX.md) lesen je eine Checkbox und liefern einen AX-Pegel (`PERMIT_OUT`).
2. `ASSEMBLE_AD_FROM_AX` setzt die Pegel zu einem DWORD zusammen (`BIT_00` und `BIT_01`).
3. `AD_TO_AD2` wandelt in den bidirektionalen AD2-Adapter, der an `K` ausgegeben wird.

Rückrichtung (Consumer → VT):

1. Das über `K` zurückgemeldete Echo erscheint an `AD_TO_AD2.AD_OUT`.
2. `SPLIT_AD_INTO_AX` zerlegt das DWORD in `BIT_00` und `BIT_01`.
3. Zwei Instanzen von [`AX_TO_Checkbox`](./AX_TO_Checkbox.md) schreiben die Bits auf die jeweilige Checkbox.

Initialisierung und Qualität:

1. `INIT` → `Checkbox_IDA_TO_AX.INIT`; `Checkbox_IDA_TO_AX.INITO` → `Checkbox_IDA_TO_AX_1.INIT`; `Checkbox_IDA_TO_AX_1.INITO` → `AX_TO_Checkbox.INIT`; `AX_TO_Checkbox.INITO` → `AX_TO_Checkbox_1.INIT`; `AX_TO_Checkbox_1.INITO` → `INITO` und → `AND_QO.REQ`.
2. `QI` → `.QI` aller vier Unterbausteine; deren `QO` → `AND_QO.IN1` bis `AND_QO.IN4` (`Checkbox_IDA_TO_AX`, `Checkbox_IDA_TO_AX_1`, `AX_TO_Checkbox`, `AX_TO_Checkbox_1`); `AND_QO.OUT` → `QO`.

`u16ObjId_1` und `u16ObjId_2` gehen jeweils sowohl an die `Checkbox_IDA_TO_AX`- als auch an die `AX_TO_Checkbox`-Instanz der betreffenden Checkbox.

## Technische Besonderheiten

- Ausgelagert aus `Uebung_094b5_AX` zur Wiederverwendung; ausgelegt für N=2 Bits/Checkboxen, analog zu `AOption_AD_DEMUX_2`.
- Speichert nicht selbst; die Persistierung obliegt dem Consumer.

## Zusammenfassung

Bündelt zwei VT-Checkboxen als Bitmaske auf einen bidirektionalen AD2-Plug und spiegelt das Consumer-Echo auf die Checkboxen zurück.

## 🛠️ Zugehörige Übungen

- [Uebung_094b5b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b5b_AX/)

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

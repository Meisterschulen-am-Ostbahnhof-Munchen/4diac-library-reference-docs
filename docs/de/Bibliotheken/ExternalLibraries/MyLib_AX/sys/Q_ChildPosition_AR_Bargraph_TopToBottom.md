# Q_ChildPosition_AR_Bargraph_TopToBottom

![Q_ChildPosition_AR_Bargraph_TopToBottom_network](./Q_ChildPosition_AR_Bargraph_TopToBottom_network.svg)

* * * * * * * * * *

## Einleitung

`Q_ChildPosition_AR_Bargraph_TopToBottom` ist ein **Bargraph Typ 3 (von oben nach unten)**: Der AR-Eingang wird mit `ScalePx` skaliert und als vertikale Position des Child-Objekts im VT ausgegeben (`X = 0`, `Y = Wert × ScalePx`). Ein größerer Wert schiebt das Child damit nach unten. Im Gegensatz zu [`Q_ChildPosition_AR_Bargraph_BottomToTop`](./Q_ChildPosition_AR_Bargraph_BottomToTop.md) erfolgt keine Spiegelung.

![Q_ChildPosition_AR_Bargraph_TopToBottom](Q_ChildPosition_AR_Bargraph_TopToBottom.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierung (an `QPos.INIT`)

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung (von `QPos.INITO`)
- **CNF**: Bestätigung des angeforderten Dienstes (von `QPos.CNF`)

### **Daten-Eingänge**

- **u16ObjId** (`UINT`, Vorgabe `ID_NULL`): Object-ID des Child-Objekts
- **u16ObjIdParent** (`UINT`, Vorgabe `ID_NULL`): Object-ID des Parent-Objekts
- **xScale** (`BOOL`, Vorgabe `TRUE`): `FALSE` = Bypass; `TRUE` = Skalierung mit dem DM/SKM-Faktor für `u16ObjIdParent`
- **ScalePx** (`REAL`, Vorgabe `1.0`): Pixel pro Werteinheit (Multiplikationsfaktor)

### **Daten-Ausgänge**

- **STATUS** (`STRING`): Dienststatus (Durchreichung vom inneren `Q_ChildPosition`)
- **s16OldXposition** (`INT`): Alte X-Position (Durchreichung)
- **s16OldYposition** (`INT`): Alte Y-Position (Durchreichung)
- **s16result** (`INT`): Rückgabewert (Durchreichung, siehe `Q_ChildPosition`)

### **Adapter**

- **rPhys** (Socket, `adapter::types::unidirectional::AR`): Physikalischer `REAL`-Eingangswert (wird skaliert und auf die Y-Position abgebildet)

## Verwendete Funktionsbausteine (FBs)

- **AR_R_MUL**: SubApp `MyLib::sys::AR_R_MUL` – multipliziert den Wert mit `ScalePx` (`FACTOR`)
- **QPos**: SubApp `MyLib::sys::Q_ChildPosition_AR_Vertikal` – setzt die vertikale Child-Position

## Programmablauf und Verbindungen

1. `rPhys` → `AR_R_MUL.IN1`; `ScalePx` → `AR_R_MUL.FACTOR`.
2. `AR_R_MUL.OUT` → `QPos.rPhys`.
3. `INIT` → `QPos.INIT`; `QPos.INITO` → `INITO`; `QPos.CNF` → `CNF`.
4. `u16ObjId`, `u16ObjIdParent`, `xScale` → `QPos`; `STATUS`, `s16OldXposition`, `s16OldYposition`, `s16result` von `QPos` nach außen.

## Technische Besonderheiten

- Keine Spiegelung: Da die VT-Y-Koordinate von oben nach unten wächst, entspricht ein größerer Wert direkt einer weiter unten liegenden Position.
- `xScale` hat hier die Vorgabe `TRUE`.
- Version 1.0 (2026-09-27): `AR_R_MUL` + `Q_ChildPosition_AR_Vertikal`

## Anwendungsszenarien

- Senkrechter Balken/Anzeige, der von oben nach unten wächst

## Fazit

`Q_ChildPosition_AR_Bargraph_TopToBottom` bildet einen AR-Wert über Skalierung direkt auf die vertikale Position eines VT-Child-Objekts ab.

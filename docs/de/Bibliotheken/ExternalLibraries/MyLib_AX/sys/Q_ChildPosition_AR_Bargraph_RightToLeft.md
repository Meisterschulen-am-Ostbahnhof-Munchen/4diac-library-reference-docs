# Q_ChildPosition_AR_Bargraph_RightToLeft

![Q_ChildPosition_AR_Bargraph_RightToLeft_network](./Q_ChildPosition_AR_Bargraph_RightToLeft_network.svg)

* * * * * * * * * *

## Einleitung

`Q_ChildPosition_AR_Bargraph_RightToLeft` ist ein Bargraph vom Typ 2 (von rechts nach links): Der `AR`-Eingang wird mit `ScalePx` skaliert, gegen `LengthPx` gespiegelt und als horizontale Position eines Kindobjekts im VT gesetzt (`X = LengthPx - (Wert × ScalePx)`, `Y = 0`). Gegenstück zu [`Q_ChildPosition_AR_Bargraph_LeftToRight`](./Q_ChildPosition_AR_Bargraph_LeftToRight.md).

![Q_ChildPosition_AR_Bargraph_RightToLeft](Q_ChildPosition_AR_Bargraph_RightToLeft.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Service-Initialisierung (durchgereicht an `QPos.INIT`)

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung (von `QPos.INITO`)
- **CNF**: Bestätigung der angeforderten Positionierung (von `QPos.CNF`)

### **Daten-Eingänge**

- **QI** (`BOOL`, Default `TRUE`): Quality Input – TRUE gibt den Dienst frei (durchgereicht an `QPos.QI`)
- **u16ObjId** (`UINT`): Object ID des Kindobjekts (Default `ID_NULL`)
- **u16ObjIdParent** (`UINT`): Object ID des Elternobjekts (Default `ID_NULL`)
- **xScale** (`BOOL`): `FALSE` = Bypass; `TRUE` (Default) = Skalierung mit dem DM/SKM-Faktor für `u16ObjIdParent`
- **ScalePx** (`REAL`): Pixel pro Werteinheit (Multiplikationsfaktor), Default `REAL#1.0`
- **LengthPx** (`REAL`): Gesamtlänge des Bargraphs in Pixeln (zur Spiegelung), Default `REAL#100.0`

### **Daten-Ausgänge**

- **QO** (`BOOL`): Quality Output – TRUE, wenn der Dienst erfolgreich ausgeführt wurde (von `QPos.QO`)
- **STATUS** (`STRING`): Service-Status, durchgereicht vom internen `Q_ChildPosition`
- **s16OldXposition** (`INT`): alte X-Position, durchgereicht
- **s16OldYposition** (`INT`): alte Y-Position, durchgereicht
- **s16result** (`INT`): Rückgabewert, durchgereicht vom internen `Q_ChildPosition`

### **Adapter**

- **rPhys** (Socket, `adapter::types::unidirectional::AR`): physikalischer `REAL`-Wert; wird skaliert, gespiegelt und auf die X-Position abgebildet

## Verwendete Funktionsbausteine (FBs)

- **`AR_R_MUL`** (SubApp `MyLib::sys::AR_R_MUL`, [AR_R_MUL](./AR_R_MUL.md)): multipliziert den Wert mit `ScalePx`
- **`SubFlip`** (SubApp `MyLib::sys::R_AR_SUB`): zieht den skalierten Wert von `LengthPx` ab (`MINUEND = LengthPx`)
- **`QPos`** (SubApp `MyLib::sys::Q_ChildPosition_AR_Horizontal`, [Q_ChildPosition_AR_Horizontal](./Q_ChildPosition_AR_Horizontal.md)): setzt die horizontale Position des Kindobjekts

## Funktionsweise

- `rPhys` → `AR_R_MUL.IN1`; `ScalePx` → `AR_R_MUL.FACTOR`
- `AR_R_MUL.OUT` → `SubFlip.IN`; `LengthPx` → `SubFlip.MINUEND`
- `SubFlip.OUT` → `QPos.rPhys`
- `INIT` → `QPos.INIT`; `QPos.INITO` → `INITO`; `QPos.CNF` → `CNF`
- `QI` → `QPos.QI`; `QPos.QO` → `QO`
- `u16ObjId`, `u16ObjIdParent`, `xScale` → gleichnamige Eingänge von `QPos`
- `QPos.STATUS`, `QPos.s16OldXposition`, `QPos.s16OldYposition`, `QPos.s16result` → gleichnamige Ausgänge

## Technische Besonderheiten

- Die Spiegelung (`LengthPx - Wert × ScalePx`) lässt den Balken von rechts nach links wachsen
- Alle Status- und Positionsausgänge sind reine Durchreichungen des internen `Q_ChildPosition`

## Anwendungsszenarien

- Horizontale Balkenanzeige im VT, die von rechts nach links wächst (z. B. Füllstand, Auslastung)

## Zusammenfassung

`Q_ChildPosition_AR_Bargraph_RightToLeft` bildet einen `AR`-Wert gespiegelt auf die X-Position eines VT-Kindobjekts ab.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

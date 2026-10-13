# Q_ChildPosition_AR_Bargraph_LeftToRight

![Q_ChildPosition_AR_Bargraph_LeftToRight_network](./Q_ChildPosition_AR_Bargraph_LeftToRight_network.svg)

* * * * * * * * * *

## Einleitung

`Q_ChildPosition_AR_Bargraph_LeftToRight` realisiert einen Bargraph vom Typ 0 (von links nach rechts): Der AR-Eingang `rPhys` wird mit `ScalePx` (Pixel pro Werteinheit) skaliert und positioniert das Kind-Objekt horizontal (X = Wert × `ScalePx`, Y = 0).

![Q_ChildPosition_AR_Bargraph_LeftToRight](Q_ChildPosition_AR_Bargraph_LeftToRight.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT**: Service-Initialisierung (`EInit`)

### **Ereignis-Ausgänge**

- **INITO**: Initialisierungs-Bestätigung (`EInit`)
- **CNF**: Bestätigung des angeforderten Service

### **Daten-Eingänge**

- **u16ObjId** (UINT): Object ID des Kind-Objekts (Default `ID_NULL`)
- **u16ObjIdParent** (UINT): Object ID des Eltern-Objekts (Default `ID_NULL`)
- **xScale** (BOOL): FALSE = Bypass; TRUE (Default) = mit dem DM/SKM-Faktor für `u16ObjIdParent` skalieren
- **ScalePx** (REAL): Pixel pro Werteinheit (Multiplikationsfaktor), Default `REAL#1.0`

### **Daten-Ausgänge**

- **STATUS** (STRING): Service-Status, durchgereicht vom internen `Q_ChildPosition`
- **s16OldXposition** (INT): alte X-Position, durchgereicht
- **s16OldYposition** (INT): alte Y-Position, durchgereicht
- **s16result** (INT): Rückgabewert, durchgereicht (siehe `Q_ChildPosition`)

### **Adapter**

- **rPhys** (Socket, `adapter::types::unidirectional::AR`): physikalischer REAL-Wert (wird skaliert und auf die X-Position abgebildet)

## Funktionsweise

Das Netzwerk besteht aus zwei SubApps:

- `AR_R_MUL` ([`MyLib::sys::AR_R_MUL`](AR_R_MUL.md)): `rPhys` → `AR_R_MUL.IN1`, `ScalePx` → `AR_R_MUL.FACTOR`; multipliziert den AR-Wert mit dem Faktor.
- `QPos` (`MyLib::sys::Q_ChildPosition_AR_Horizontal`): `AR_R_MUL.OUT` → `QPos.rPhys`; setzt die X-Position des Kind-Objekts über den AR-Adapter (Y fest 0).

Die Ereignisse `INIT`, `INITO` und `CNF` sowie `u16ObjId`, `u16ObjIdParent`, `xScale` und die vier Ausgänge werden direkt von bzw. nach `QPos` durchgereicht.

## Technische Besonderheiten

- Typ 0 (LeftToRight) der Bargraph-Varianten: Die Position wächst mit dem Wert in X-Richtung, Y bleibt 0
- `xScale` hat hier den Default TRUE (Skalierung mit DM/SKM-Faktor), im Gegensatz zu [`Q_ChildPosition_AR_Vertikal`](Q_ChildPosition_AR_Vertikal.md) mit Default FALSE
- Zustandslos auf SubApp-Ebene; alle Statusausgänge stammen vom inneren `Q_ChildPosition`

## Anwendungsszenarien

- Balkenanzeige, bei der ein Kind-Objekt proportional zu einem Messwert horizontal verschoben wird

## Fazit

`Q_ChildPosition_AR_Bargraph_LeftToRight` skaliert einen AR-Wert mit `ScalePx` und setzt damit die X-Position eines Kind-Objekts für einen Links-nach-rechts-Bargraph.

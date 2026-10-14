# AX_SwitchPic_2_1

![AX_SwitchPic_2_1_network](./AX_SwitchPic_2_1_network.svg)

* * * * * * * * * *

## Einleitung

`AX_SwitchPic_2_1` schaltet das Ziel eines VT-**ObjectPointers** (Bild) zwischen zwei Zuständen um. Der Selektor kommt als **AX-Adapter** statt als `BOOL`-Eingangsvariable (AX-Pendant zu `MyLib::sys::SwitchPic_2_1`): Bei jedem Update von `DI1` wird die per `DI1.D1` gewählte Ziel-Object-ID (`pictures.down` bei `TRUE`, `pictures.up` bei `FALSE`) auf den ObjectPointer `Picture` geschrieben. Ein externer Trigger ist nicht nötig, da das eigene `E1`/`D1` von `DI1` direkt speist. Die Initialisierung läuft über `INIT`/`INITO` des inneren `Q_NumericValue` (Quality-Muster mit `QI`/`QO`).

![AX_SwitchPic_2_1](AX_SwitchPic_2_1.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung – an `Q_NumericValue.INIT`; weitere Ereignisse kommen über den Adapter-Socket

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung – von `Q_NumericValue.INITO`

### **Daten-Eingänge**

- **QI** (`BOOL`, Vorgabe `TRUE`): Quality Input – TRUE, um den Dienst freizugeben (an `Q_NumericValue.QI`)
- **Picture** (`UINT`): Object-ID des ObjectPointers, in den geschrieben wird
- **pictures** (`isobus::UT::Q::types::s2x1ObjectIDs`): Die beiden Ziel-Object-IDs (`up`, `down`); Vorbelegung: beide `ID_NULL`

### **Daten-Ausgänge**

- **QO** (`BOOL`): Quality Output – TRUE, wenn der Dienst erfolgreich ausgeführt wurde (von `Q_NumericValue.QO`)

### **Adapter**

- **DI1** (Socket): Unidirektionaler Adapter vom Typ `adapter::types::unidirectional::AX` – Selektor: `down` (`pictures.down`) bei `TRUE`, `up` (`pictures.up`) bei `FALSE`. Jedes Update schreibt sofort neu.

## Verwendete Funktionsbausteine (FBs)

- **F_MOVE**: `iec61131::selection::F_MOVE` (`DataType=isobus::UT::Q::types::s2x1ObjectIDs`) – kopiert `pictures` auf `OUT.up.u16ObjId`/`OUT.down.u16ObjId`
- **AX_X_TO_BOOL**: `adapter::conversion::unidirectional::AX_X_TO_BOOL` – wandelt den AX-Selektor in Ereignis `CNF` und Wert `IN` (`BOOL`)
- **F_MUX_2**: `iec61131::selection::F_SEL` – wählt `IN0` (`up`) bei `G=FALSE`, `IN1` (`down`) bei `G=TRUE`
- **Q_NumericValue**: `isobus::UT::Q::Q_NumericValue` – schreibt den gewählten Wert (`u32NewValue`) in das VT-Objekt `u16ObjId`

## Programmablauf und Verbindungen

1. `INIT` → `Q_NumericValue.INIT`; `Q_NumericValue.INITO` → `INITO` und → `F_MOVE.REQ`; `F_MOVE.CNF` → `F_MUX_2.REQ` (Initialisierung der Ziel-Object-IDs).
2. `DI1` → `AX_X_TO_BOOL.AX_IN`; `AX_X_TO_BOOL.CNF` → `F_MUX_2.REQ`, `AX_X_TO_BOOL.IN` → `F_MUX_2.G`.
3. `F_MOVE.OUT.up.u16ObjId` → `F_MUX_2.IN0`; `F_MOVE.OUT.down.u16ObjId` → `F_MUX_2.IN1`.
4. `F_MUX_2.CNF` → `Q_NumericValue.REQ`; `F_MUX_2.OUT` → `Q_NumericValue.u32NewValue`; `Picture` → `Q_NumericValue.u16ObjId`.
5. `QI` → `Q_NumericValue.QI`; `Q_NumericValue.QO` → `QO`.

## Technische Besonderheiten

- Im Gegensatz zu `SwitchPic_2_1` (dort `DI1: BOOL` plus externes `EI`-Ereignis) benötigt die AX-Variante keinen externen Trigger.
- Das Ziel `ID_NULL` kann z. B. genutzt werden, um einen ObjectPointer „leer“ zu schalten.

## Anwendungsszenarien

- Umschalten eines ObjectPointers zwischen z. B. `Rectangle_Aktiv` und `ID_NULL` je nach Betriebszustand (z. B. Anzeige für Elevatorband, Igelband, Befüllband)

## Fazit

`AX_SwitchPic_2_1` schreibt bei jedem Update eines AX-Selektors eine von zwei konfigurierbaren Object-IDs auf einen VT-ObjectPointer.

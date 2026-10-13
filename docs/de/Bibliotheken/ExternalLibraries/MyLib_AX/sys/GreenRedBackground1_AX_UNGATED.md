# GreenRedBackground1_AX_UNGATED

![GreenRedBackground1_AX_UNGATED_network](./GreenRedBackground1_AX_UNGATED_network.svg)

* * * * * * * * * *

## Einleitung

`GreenRedBackground1_AX_UNGATED` setzt die **Hintergrundfarbe** eines VT-Objekts über einen AX-Adapter: `TRUE` → Grün, `FALSE` → Rot. „UNGATED“ bedeutet, dass der Selektor nicht durch ein zusätzliches Freigabesignal gegatet wird – jedes Update von `DI1` schreibt die Farbe sofort.

![GreenRedBackground1_AX_UNGATED](GreenRedBackground1_AX_UNGATED.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

*Keine direkten Ereignis-Eingänge vorhanden – Ereignisse kommen über den Adapter-Socket*

### **Ereignis-Ausgänge**

- **CNF**: Ausführungsbestätigung (von `Q_BackgroundColour.CNF`)

### **Daten-Eingänge**

- **u16ObjId** (`UINT`, Vorgabe `ID_NULL`): Object-ID des Softkeys/Buttons

### **Daten-Ausgänge**

- **STATUS_1** (`STRING`): Dienststatus
- **u8OldColour_1** (`USINT`): Alter Farbwert des Objekts
- **result_1** (`INT`): Rückgabewert (siehe `Q_BackgroundColour`)

### **Adapter**

- **DI1** (Socket, `adapter::types::unidirectional::AX`): Selektor – `TRUE` für Grün, `FALSE` für Rot

## Verwendete Funktionsbausteine (FBs)

- **AX_X_TO_BOOL**: `adapter::conversion::unidirectional::AX_X_TO_BOOL` – wandelt den AX-Selektor in Ereignis `CNF` und Wert `IN` (`BOOL`)
- **AX_SEL**: `iec61131::selection::F_SEL` mit `IN0 = COLOR_RED`, `IN1 = COLOR_GREEN` – wählt die Farbe
- **Q_BackgroundColour**: `isobus::UT::Q::Q_BackgroundColour` – schreibt die Hintergrundfarbe auf das VT-Objekt

## Programmablauf und Verbindungen

1. `DI1` → `AX_X_TO_BOOL.AX_IN`; `AX_X_TO_BOOL.IN` → `AX_SEL.G`; `AX_X_TO_BOOL.CNF` → `AX_SEL.REQ`.
2. `AX_SEL.OUT` → `Q_BackgroundColour.u8Colour`; `AX_SEL.CNF` → `Q_BackgroundColour.REQ`.
3. `u16ObjId` → `Q_BackgroundColour.u16ObjId`.
4. `Q_BackgroundColour.CNF` → `CNF`; `STATUS`, `u8OldColour`, `s16result` → `STATUS_1`, `u8OldColour_1`, `result_1`.

## Anwendungsszenarien

- Grün/Rot-Statusanzeige für Buttons oder Flächen (z. B. Betrieb/Störung) ohne zusätzliche Freigabelogik

## Fazit

`GreenRedBackground1_AX_UNGATED` färbt ein VT-Objekt abhängig von einem AX-Signal sofort grün oder rot.

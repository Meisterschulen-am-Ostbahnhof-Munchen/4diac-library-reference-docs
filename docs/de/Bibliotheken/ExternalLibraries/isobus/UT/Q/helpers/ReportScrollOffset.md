# ReportScrollOffset

* * * * * * * * * *

## Einleitung

`ReportScrollOffset` ist ein Hilfs-Service-Interface-Baustein aus dem Paket `isobus::UT::Q::helpers`. Er sitzt innerhalb der [ScrollFS](../ScrollFS.md)-FBNetwork-Struktur (parallel zu `MoveList` und `ListY`) und meldet die aktuelle Scroll-Position (Zeilen-Index `i32Pos` × Zeilenhöhe `i32RowHeight` in Pixeln) für den Container `u16ContainerId` an das ECU-Sichtbarkeits-Gate (`VtMaskVisibility`).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `INIT`: Initialisierung des Service-Interfaces (`With u16ContainerId`).
- `REQ`: Übermittelt eine Positionsänderung (`With i32Pos`, `With i32RowHeight`).

### **Ereignis-Ausgänge**

- `INITO`: Bestätigt die Initialisierung.
- `CNF`: Bestätigt die Positionsmeldung.

### **Daten-Eingänge**

- `u16ContainerId` (UINT): Objekt-ID des `*_Scrolling_Content`-Containers.
- `i32Pos` (DINT): Aktuelle Zeilen-Position (0…PosMax).
- `i32RowHeight` (DINT): Zeilenhöhe in Pixeln zur Umrechnung in den Pixel-Offset.

### **Daten-Ausgänge**

Keine Daten-Ausgänge.

### **Adapter**

Keine Adapter vorhanden.

## Funktionsweise

1. Bei `INIT` sperrt der Baustein die Container-ID `u16ContainerId`.
2. Bei jedem `REQ`-Event berechnet der Baustein `PixelOffset := i32Pos * i32RowHeight` und leitet diesen an das Sichtbarkeits-Gate der ECU weiter (`VtMaskVisibility_OnScroll`).
3. Das ECU-Gate nutzt diese Information, um VT-Aktualisierungen (z. B. `Q_*`-Kommandos) auf derzeit sichtbare Zeilen zu begrenzen.

## Siehe auch

- [ScrollFS](../ScrollFS.md)
- [ScrollFS_PHYS_Softkey](../ScrollFS_PHYS_Softkey.md)
- [ScrollFS_PHYS_Button](../ScrollFS_PHYS_Button.md)

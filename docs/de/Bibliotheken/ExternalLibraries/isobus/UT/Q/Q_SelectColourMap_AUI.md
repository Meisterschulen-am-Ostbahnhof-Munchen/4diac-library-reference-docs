# Q_SelectColourMap_AUI

![Q_SelectColourMap_AUI](./Q_SelectColourMap_AUI.svg)

* * * * * * * * * *

## Einleitung

Der **Q_SelectColourMap_AUI** ist ein AUI‑Adapter‑Wrapper für den Baustein **Q_SelectColourMap** (ISO 11783‑6, Teil 6 – F.60). Er ermöglicht die Auswahl der aktiven Farbpalette / Colour Map auf einem ISOBUS Terminal über AUI‑Adapter‑Schnittstellen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
|---|---|---|
| `INIT` | `EInit` | Service‑Initialisierung |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `INITO` | `EInit` | Bestätigung der Initialisierung |
| `CNF` | `Event` | Bestätigung des angeforderten Dienstes |

### **Daten-Eingänge**

Keine direkten Dateneingänge vorhanden – die Farbpaletten-IDs werden über die AUI-Adapter übertragen.

### **Daten-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `STATUS` | `STRING` | Dienststatus |
| `s16result` | `INT` | Rückgabewert des Befehls |

### **Adapter**

| Typ | Name | Richtung | Kommentar |
|---|---|---|---|
| `adapter::types::unidirectional::AUI` | `u16ObjIdColourMap` | Socket (Eingang) | Neue Farbpaletten-Object-ID (39000–39999 für ColourMap, 45000–45999 für ColourPalette, ID_NULL / 0xFFFF für Standard-Farbtabelle) |
| `adapter::types::unidirectional::AUI` | `u16OldObjIdColourMap` | Plug (Ausgang) | Bisherige Farbpaletten-Object-ID |

## Funktionsweise

Der Baustein kapselt den inneren FB `Q_SelectColourMap` (*isobus::UT::Q::Q_SelectColourMap*).
Bei einem Ereignis an `u16ObjIdColourMap.E1` wird das `REQ`-Ereignis des internen FBs mit der neuen ID aus `u16ObjIdColourMap.D1` aufgerufen. Nach erfolgreicher Umschaltung wird über `u16OldObjIdColourMap.E1` und `CNF` die alte ID (`u16OldObjIdColourMap.D1`) ausgegeben.

## Technische Besonderheiten

- **AUI-Adapter-Kopplung:** Adapterbasierte Umschaltung von Farbpaletten über den Socket `u16ObjIdColourMap`.
- **Wertebereiche gemäß ISO 11783-6 F.60:**
  - `ColourMap`: 39000–39999 (ab VT Version 4)
  - `ColourPalette`: 45000–45999 (ab VT Version 6)
  - `ID_NULL` (`0xFFFF` / 65535): Wiederherstellen der Standard-Farbtabelle.
- **Implementierungseinschränkungen (VTClientHelper):**
  - `VTClientHelper::iso_is_colour_map_id` akzeptiert aktuell nur den Bereich 39000–39999 (Farbpaletten-IDs 45000–45999 werden abgelehnt).
  - `cmd_select_colour_map_or_palette` verschluckt `ID_NULL` (`0xFFFF`), gibt `VT_E_NO_ERR` (0) zurück und sendet keinen Befehl an den VT, weshalb das Wiederherstellen der Standard-Farbtabelle derzeit nicht funktioniert.

## Zustandsübersicht

Delegiert alle Aktionen an `Q_SelectColourMap`.

## Anwendungsszenarien

- Dynamische Umschaltung von Farbschemata (z. B. Tag-/Nachtmodus) im ISOBUS Terminal per AUI-Adapter.

## Vergleich mit ähnlichen Bausteinen

Erweitert `Q_SelectColourMap` um AUI-Adapter-Sockets und -Plugs zur einfachen Einbindung in adapterbasierte Architekturen.

## Fazit

**Q_SelectColourMap_AUI** ist der adapterbasierte Wrapper für Farbpaletten-Umschaltungen in ISOBUS VT-Anwendungen.

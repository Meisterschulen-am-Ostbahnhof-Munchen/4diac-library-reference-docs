# Q_ExecuteExtendedMacro_AUI

![Q_ExecuteExtendedMacro_AUI](./Q_ExecuteExtendedMacro_AUI.svg)

* * * * * * * * * *

## Einleitung

Der **Q_ExecuteExtendedMacro_AUI** ist ein AUI‑Adapter‑Wrapper für den Baustein **Q_ExecuteExtendedMacro** (ISO 11783‑6, Teil 6 – F.62). Er ermöglicht das Ausführen von Extended‑Makros (16-Bit Makro-ID 1–999) auf einem ISOBUS Terminal über eine unidirektionale AUI‑Schnittstelle.

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

Keine direkten Dateneingänge vorhanden – die Extended‑Makro‑ID wird über den AUI‑Adapter‑Socket empfangen.

### **Daten-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `STATUS` | `STRING` | Dienststatus |
| `s16result` | `INT` | Rückgabewert des Befehls |

### **Adapter**

| Typ | Name | Richtung | Kommentar |
|---|---|---|---|
| `adapter::types::unidirectional::AUI` | `u16ObjId` | Socket (Eingang) | Object-ID des Extended-Makros (1–999) |

## Funktionsweise

Verbindet den internen FB `Q_ExecuteExtendedMacro` (*isobus::UT::Q::Q_ExecuteExtendedMacro*) mit der AUI‑Adapter‑Schnittstelle.
Ein Ereignis an `u16ObjId.E1` löst `REQ` am internen FB aus, der die 16-Bit Makro‑ID aus `u16ObjId.D1` an das Terminal übermittelt.

## Technische Besonderheiten

- **16‑Bit Extended-Makro-Bereich:** Unterstützt Makro‑IDs von 1 bis 999 (ISO 11783‑6 VT Version 5+).
- **Adapter‑Wrapper:** Unidirektionale AUI-Anbindung.

## Zustandsübersicht

Delegiert alle Aktionen an den inneren FB `Q_ExecuteExtendedMacro`.

## Anwendungsszenarien

- Ausführen großer Extended-Makros in VT Version 5+ Umgebungen per AUI-Adapter.

## Vergleich mit ähnlichen Bausteinen

Für Standard-Makros (IDs 1–255) steht **Q_ExecuteMacro_AUI** zur Verfügung; für 16-Bit Makro-IDs (1–999) wird **Q_ExecuteExtendedMacro_AUI** eingesetzt.

## Fazit

**Q_ExecuteExtendedMacro_AUI** ist der standardisierte AUI-Wrapper zur Ausführung von Extended-Makros in ISOBUS VT Version 5+ Systemen.

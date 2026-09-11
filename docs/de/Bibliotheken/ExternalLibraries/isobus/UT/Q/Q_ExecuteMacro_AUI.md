# Q_ExecuteMacro_AUI

![Q_ExecuteMacro_AUI](./Q_ExecuteMacro_AUI.svg)

* * * * * * * * * *

## Einleitung

Der **Q_ExecuteMacro_AUI** ist ein AUI‑Adapter‑Wrapper für den Baustein **Q_ExecuteMacro** (ISO 11783‑6, Teil 6 – F.48). Er ermöglicht das Ausführen von Makros auf einem ISOBUS Terminal über eine unidirektionale AUI‑Schnittstelle (UINT). Der Baustein kapselt den internen `Q_ExecuteMacro`‑Kern und bietet eine einfache, adapterbasierte Anbindung.

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

Keine direkten Dateneingänge vorhanden – die Makro‑Object‑ID wird über den AUI‑Adapter‑Socket empfangen.

### **Daten-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `STATUS` | `STRING` | Dienststatus |
| `s16result` | `INT` | Rückgabewert des Befehls |

### **Adapter**

| Typ | Name | Richtung | Kommentar |
|---|---|---|---|
| `adapter::types::unidirectional::AUI` | `u16ObjId` | Socket (Eingang) | Object-ID des auszuführenden Makros (1–255) |

## Funktionsweise

Der Baustein verbindet intern den Funktionsblock `Q_ExecuteMacro` (*isobus::UT::Q::Q_ExecuteMacro*) mit der AUI‑Adapter‑Schnittstelle.
Beim Eintreffen eines Ereignisses am Socket `u16ObjId.E1` wird das Ereignis `REQ` des internen FBs ausgelöst und die Makro‑ID aus `u16ObjId.D1` übergeben. Nach der Ausführung sendet der interne FB eine Bestätigung über `CNF` zurück und aktualisiert `STATUS` sowie `s16result`.

## Technische Besonderheiten

- **Adapter‑Wrapper:** Sämtliche Eingabedaten werden über den AUI‑Socket empfangen.
- **8‑Bit-Makro-Bereich:** `Q_ExecuteMacro` verarbeitet Makro‑IDs im Bereich 1–255.
- **ISOBUS‑Konformität:** Gemäß ISO 11783‑6 Annex F.48 spezifiziert.

## Zustandsübersicht

Der Wrapper selbst delegiert alle Zustände an den inneren FB `Q_ExecuteMacro`.

1. **Initialisierung** – `INIT` initialisiert den Baustein und meldet `INITO`.
2. **Befehlsausführung** – Ereignis an `u16ObjId.E1` stößt Makro-Ausführung an.
3. **Bestätigung** – `CNF` meldet den Ausführungsstatus.

## Anwendungsszenarien

- Ausführen von VT-Makros per AUI-Adapter in ISO 11783-6 Steuerungen.
- Automatisierte Makro-Ausführung bei Ereignissen oder Sequenzschritten.

## Vergleich mit ähnlichen Bausteinen

Im Vergleich zum direkten `Q_ExecuteMacro` bietet **Q_ExecuteMacro_AUI** eine adapterbasierte Anbindung. Für Makro‑IDs > 255 wird der Baustein **Q_ExecuteExtendedMacro_AUI** verwendet.

## Fazit

**Q_ExecuteMacro_AUI** ist ein praktischer Adapter‑Wrapper zur Ausführung von ISOBUS-VT-Makros über AUI-Schnittstellen.

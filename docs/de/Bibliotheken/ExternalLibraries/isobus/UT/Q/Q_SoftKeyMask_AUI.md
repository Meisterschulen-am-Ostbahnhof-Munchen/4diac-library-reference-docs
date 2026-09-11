# Q_SoftKeyMask_AUI

![Q_SoftKeyMask_AUI](./Q_SoftKeyMask_AUI.svg)

* * * * * * * * * *

## Einleitung

Der **Q_SoftKeyMask_AUI** ist ein AUI‑Adapter‑Wrapper für den Baustein **Q_SoftKeyMask** (ISO 11783‑6, Teil 6 – F.36). Er ermöglicht das Umschalten der Softkey-Maske auf einem ISOBUS Terminal über AUI‑Adapter‑Schnittstellen.

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

| Name | Typ | Kommentar |
|---|---|---|
| `u8MaskType` | `USINT` | Maskentyp (0: DataMask, 1: AlarmMask) |
| `u16DataMaskId` | `UINT` | Object-ID der zugehörigen Data/AlarmMask |

### **Daten-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `STATUS` | `STRING` | Dienststatus |
| `s16result` | `INT` | Rückgabewert des Befehls |

### **Adapter**

| Typ | Name | Richtung | Kommentar |
|---|---|---|---|
| `adapter::types::unidirectional::AUI` | `u16SoftKeyMaskId` | Socket (Eingang) | Neue Softkey-Masken-ID |
| `adapter::types::unidirectional::AUI` | `u16OldSoftKeyMaskId` | Plug (Ausgang) | Bisherige Softkey-Masken-ID |

## Funktionsweise

Der Baustein kapselt den internen FB `Q_SoftKeyMask` (*isobus::UT::Q::Q_SoftKeyMask*).
Ein Ereignis an `u16SoftKeyMaskId.E1` löst `REQ` am internen FB aus, der die Maskenumschaltung initiiert. Nach der Ausführung wird die alte Softkey-Masken-ID über `u16OldSoftKeyMaskId.D1` und `u16OldSoftKeyMaskId.E1` ausgegeben.

## Technische Besonderheiten

- **Adapter-basierte Softkey-Steuerung:** Flexibles Umschalten von Softkey-Layouts per AUI-Adapter.
- **ISOBUS-Konformität:** Basiert auf ISO 11783-6 F.36.

## Zustandsübersicht

Delegiert alle Aktionen an `Q_SoftKeyMask`.

## Anwendungsszenarien

- Dynamisches Wechseln von Softkey-Belegungen in ISOBUS Terminals über AUI-Adapter.

## Vergleich mit ähnlichen Bausteinen

Bietet im Vergleich zu `Q_SoftKeyMask` eine unidirektionale AUI-Adapter-Schnittstelle.

## Fazit

**Q_SoftKeyMask_AUI** ist der adapterbasierte Wrapper für Softkey-Masken-Umschaltungen in ISOBUS-Anwendungen.

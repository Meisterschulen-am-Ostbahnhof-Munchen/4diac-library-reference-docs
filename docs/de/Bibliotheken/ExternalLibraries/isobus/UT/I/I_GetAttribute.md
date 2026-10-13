# I_GetAttribute

![I_GetAttribute](I_GetAttribute.svg)

* * * * * * * * * *

## Einleitung

`I_GetAttribute` implementiert das Kommando **Get Attribute Value** (ISO 11783-6, F.58). Es steht ab VT-Version 4 zur Verfügung und dient einem Working Set dazu, beim VT den aktuellen Zustand von Objekten im VT abzufragen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Service-Initialisierung
    - Verknüpft mit: `u16ObjId`, `u8AID`
- **REQ**: Service-Anfrage

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung
- **CNF**: Bestätigung der angeforderten Service-Anfrage
    - Verknüpft mit: `STATUS`, `s16result`

### **Daten-Eingänge**

- **u16ObjId** (`UINT`, Initialwert `ID_NULL`): Objekt-ID
- **u8AID** (`USINT`): Attribut-ID des Objekts

### **Daten-Ausgänge**

- **STATUS** (`STRING`): Service-Status
- **s16result** (`INT`): Rückgabewert (siehe unten)

### **Adapter**

Keine Adapter-Schnittstellen vorhanden.

## Funktionsweise

Das Kommando „Get Attribute Value“ (F.58) ist eine feste 8-Byte-Nachricht (Objekt-ID in Byte 2 und 3, Attribut-ID in Byte 4; kein Transport Protocol).

`INIT` prüft vor dem Senden des Kommandos sowohl den Objekttyp (`iso_has_readable_attribute_id`: besitzt dieser Objekttyp überhaupt eine Attribut-ID, schreibgeschützt oder beschreibbar?) als auch die konkrete Attribut-ID gegen die Attributtabelle dieses Typs (`iso_is_readable_attribute`). Anders als bei Change Attribute werden hier sowohl schreibgeschützte als auch beschreibbare Attribut-IDs akzeptiert. Die zugrunde liegende C-Implementierung (`cmd_get_attribute_value`) führt selbst keine Typprüfung durch, nur die `ID_NULL`-Behandlung; das VT validiert Objekt-ID und Attribut-ID zusätzlich auf seiner Seite und antwortet bei ungültigen Werten mit einer Fehlerantwort.

`ID_NULL` (65535) ist kein Kommandoziel, kann aber in `INIT` verwendet werden, um den Funktionsblock zu deaktivieren.

### Rückgabewerte (`s16result`)

| Wert | Bedeutung |
| :--- | :--- |
| `VT_E_NO_ERR` (0) | OK |
| `VT_E_DEACTIVATED` (-40) | FB über `ID_NULL` bei `INIT` deaktiviert |
| `VT_E_INVALID_OBJECT_ID` (-132) | Der Typ der Objekt-ID hat keine lesbare Attribut-ID (`iso_has_readable_attribute_id`) |
| `VT_E_INVALID_ATTRIBUTE_ID` (-133) | Attribut-ID ist für diesen Objekttyp keine gültige/lesbare AID (`iso_is_readable_attribute`) |
| `VT_E_NOT_READY` (-131) | Gepuffert: `INIT` noch nicht abgeschlossen / VT noch nicht bereit |
| `VT_E_OVERFLOW` (-6) | Pufferüberlauf |
| `VT_E_NOACT` (-8) | Kommando im aktuellen Zustand nicht möglich |
| `VT_E_NO_INSTANCE` (-21) | Kein VT-Client verfügbar |
| `VT_E_HANDLE_INVALID` (-128) | Fehlerursache: ungültiges Handle |
| `VT_E_ISO_INSTANCE_INVALID` (-129) | Kennung für Instanz (Verbindung) ungültig |
| `VT_E_NOT_ALIVE` (-130) | Kennung für Instanz gültig, aber VT tot |

## Technische Besonderheiten

- **Instanz-Eindeutigkeit**: Dieser Baustein erfordert Eindeutigkeit bezüglich des Paares (`u16ObjId`, `u8AID`). Im gesamten Programm darf nur eine Instanz von `I_GetAttribute` für dasselbe Objekt-Attribut-Paar existieren. `u8AID` wird bei `INIT` eingelesen; eine zweite Instanz auf dieselbe Objekt-ID und Attribut-ID wird bei `INIT` deaktiviert.
- Der Baustein liefert über `CNF` nur Status und Rückgabewert; den gelesenen Attributwert liefern die Service-Interface-Bausteine wie [`Attribute_IDA`](../io/Attribute/Attribute_IDA.md).

## Anwendungsszenarien

- Abfragen des aktuellen Zustands eines Objekts im VT (ab VT-Version 4)

## Fazit

`I_GetAttribute` sendet das Kommando Get Attribute Value (F.58) für ein Objekt-/Attribut-Paar und meldet Status und Rückgabewert.

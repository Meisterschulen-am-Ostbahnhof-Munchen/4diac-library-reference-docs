# I_GetAttribute

![I_GetAttribute](https://user-images.githubusercontent.com/116869307/214147879-2749e8c2-364e-4335-9c0e-0445694831e4.png)

* * * * * * * * * *

## Einleitung

Der **I_GetAttribute** ist ein standardkonformer Funktionsbaustein zum Abfragen von Objektattributen in Virtual Terminals, entwickelt unter EPL-2.0 Lizenz. Die Version 1.0 implementiert die ISO 11783-6 (Teil 6 - F.58) Spezifikation für VT-Systeme ab Version 4.

![I_GetAttribute](I_GetAttribute.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `INIT`: Initialisierungsanforderung (mit Objekt-ID)
- `REQ`: Attributabfrage-Anforderung (mit Attribut-ID)

### **Ereignis-Ausgänge**

- `INITO`: Initialisierungsbestätigung
- `CNF`: Bestätigung der Anforderung (mit `STATUS` und `s16result`)

### **Daten-Eingänge**

- `u16ObjId` (UINT): Objekt-ID (16-bit)
- `u8AID` (USINT): Attribut-ID (8-bit)

### **Daten-Ausgänge**

- `STATUS` (STRING): Betriebsstatusmeldung
- `s16result` (INT): ISO-konformer Ergebniscode (0 = OK, negative Werte = Fehler)

## Gültige Objekt-IDs & Validierung

Der Befehl *Get Attribute Value* (F.58) ist eine feste 8-Byte-Nachricht (Objekt-ID in Bytes 2,3, Attribut-ID in Byte 4; kein Transport Protocol). `INIT` validiert sowohl den Objekttyp (`iso_has_readable_attribute_id` – besitzt dieser Objekttyp überhaupt auslesbare AIDs, egal ob Read-Only oder Schreibbar?) als auch die spezifische Attribut-ID gegen die Attributtabelle des Typs (`iso_is_readable_attribute`), bevor der Befehl gesendet wird. Im Gegensatz zu *Change Attribute* werden hier sowohl Read-Only- als auch schreibbare AIDs akzeptiert.

Die zugrunde liegende C-Implementierung (`cmd_get_attribute_value`) führt selbst keine Typprüfung durch (nur `ID_NULL`-Handling). Das VT validiert zusätzlich die Objekt-ID und Attribut-ID auf seiner Seite und liefert im Fehlerfall eine Fehlerantwort zurück.

`ID_NULL` (65535) ist kein gültiges Kommandoziel, deaktiviert aber bei `INIT` den Baustein (`VT_E_DEACTIVATED`).

## Funktionsweise

1. **Initialisierung**:
   - `INIT` mit Objekt-ID (`u16ObjId`)
   - `INITO` bestätigt Betriebsbereitschaft

2. **Attributabfrage (Asynchrones Event)**:
   - `REQ` löst die Abfrage für die gewünschte Attribut-ID (`u8AID`) aus.
   - Da die Attributabfrage ein **asynchrones Event** am ISOBUS VT ist, trifft die Antwort der Ressource asynchron über ein Indikations-Event (`IND` / `Attribute_ID`) bzw. `CNF` ein.

3. **Fehlerbehandlung**:
   - ISO-standardisierte Fehlercodes in `s16result`
   - Detaillierte Statusmeldungen über `STATUS`

## Technische Besonderheiten

✔ **ISO 11783-6 konform** (F.58)
✔ **Asynchrones Event-Handling** (Antwort-Indikation über `IND` / `Attribute_ID`)
✔ **Exklusiv für VT Version 4+**
✔ **Universal einsetzbar** (Alle Objekttypen mit auslesbaren AIDs)
✔ **Echtzeitfähig** (Schnelle Abfragezyklen)

## Attribut-Typen

| Kategorie      | Beispiel-IDs | Beschreibung                |
| -------------- | ------------ | --------------------------- |
| Grundattribute | 0x01 - 0x0F  | Sichtbarkeit, Aktivität     |
| Darstellung    | 0x10 - 0x2F  | Farben, Rahmen, Ausrichtung |
| Inhalte        | 0x30 - 0x4F  | Textwerte, Numerische Werte |
| Zustände       | 0x50 - 0x6F  | Alarmstatus, Betriebsmodi   |

## Rückgabecodes (s16result)

| Code | Konstante                 | Bedeutung                |
| ---- | ------------------------- | ------------------------ |
| 0    | VT_E_NO_ERR               | Erfolgreiche Abfrage     |
| -40  | VT_E_DEACTIVATED          | Baustein deaktiviert via ID_NULL bei INIT |
| -132 | VT_E_INVALID_OBJECT_ID    | Objekttyp besitzt keine auslesbaren AIDs (`iso_has_readable_attribute_id`) |
| -133 | VT_E_INVALID_ATTRIBUTE_ID | Attribut-ID ist für diesen Objekttyp ungültig (`iso_is_readable_attribute`) |
| -131 | VT_E_NOT_READY            | Gebuffert: INIT noch nicht abgeschlossen / VT nicht bereit |
| -6   | VT_E_OVERFLOW             | Pufferüberlauf           |
| -8   | VT_E_NOACT                | Befehl im aktuellen Zustand nicht möglich |
| -21  | VT_E_NO_INSTANCE          | Kein VT-Client verfügbar |
| -128 | VT_E_HANDLE_INVALID       | Fehlerursache: Ungültiges Handle |
| -129 | VT_E_ISO_INSTANCE_INVALID | Ungültige VT-Instanz     |
| -130 | VT_E_NOT_ALIVE            | VT-Instanz gültig, aber VT tot |

## Anwendungsszenarien

- **Systemdiagnose**: Zustandsabfragen
- **Benutzerinteraktion**: Eingabewertüberprüfung
- **Automatisierung**: Regelbasierte Steuerungen
- **Konfiguration**: Parameterauslesung

## ⚖️ Vergleich mit ähnlichen Bausteinen

| Feature        | I_GetAttribute | VtReadValue | VtObjectQuery  |
| -------------- | -------------- | ----------- | -------------- |
| ISO-Standard   | ✔              | ✖           | ✖              |
| VT-Version     | 4+             | Alle        | Alle           |
| Attributbreite | Universal      | Werte-only  | Limitierte IDs |

## Fazit

Der I_GetAttribute-Baustein bietet die Standardimplementierung für Attributabfragen:

- **Effizient**: Minimale Latenzzeiten
- **Zuverlässig**: Robuste Fehlererkennung
- **Flexibel**: Unterstützt alle Objekttypen

Unverzichtbar für:

- Diagnosesysteme
- Automatisierungslösungen
- Interaktive VT-Anwendungen
- Konfigurationsmanagement

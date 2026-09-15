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

## Gültige Objekt-IDs

Der Befehl F.58 ist eine feste 8-Byte-Nachricht (Objekt-ID in Bytes 2,3, Attribut-ID in Byte 4; kein Transport Protocol) und **beschränkt den Objekttyp nicht** — jedes Objekt im Objektpool kann abgefragt werden. Das VT validiert die Objekt-ID und die Attribut-ID und meldet einen Fehler, falls eine davon ungültig ist.

ID_NULL (65535) ist kein gültiges Kommandoziel, deaktiviert aber bei `INIT` den Baustein.

## Funktionsweise

1. **Initialisierung**:
   - `INIT` mit Objekt-ID (`u16ObjId`)
   - `INITO` bestätigt Betriebsbereitschaft

2. **Attributabfrage (Asynchrones Event)**:
   - `REQ` löst die Abfrage für die gewünschte Attribut-ID (`u8AID`) aus.
   - Da die Attributabfrage ein **asynchrones Event** am ISOBUS VT ist, trifft die Antwort der Ressource asynchron über ein Indikations-Event (`IND` / `Attribute_ID`) bzw. `CNF` mit dem aktuellen 32-Bit-Attributwert `u32ValueAttribute` ein.

3. **Fehlerbehandlung**:
   - ISO-standardisierte Fehlercodes in `s16result`
   - Detaillierte Statusmeldungen über `STATUS`

## Technische Besonderheiten

✔ **ISO 11783-6 konform** (F.58)
✔ **Asynchrones Event-Handling** (Antwort-Indikation über `IND` / `Attribute_ID`)
✔ **Exklusiv für VT Version 4+**
✔ **Universal einsetzbar** (Alle Objekttypen)
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
| -6   | VT_E_OVERFLOW             | Pufferüberlauf           |
| -8   | VT_E_NOACT                | VT nicht bereit          |
| -21  | VT_E_NO_INSTANCE          | Kein VT-Client verfügbar |
| -129 | VT_E_ISO_INSTANCE_INVALID | Ungültige VT-Instanz     |
| -130 | VT_E_NOT_ALIVE            | VT nicht aktiv           |

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

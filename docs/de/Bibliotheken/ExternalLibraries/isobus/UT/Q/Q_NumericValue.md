# Q_NumericValue

![Q_NumericValue](https://user-images.githubusercontent.com/113907471/204326982-47eea33a-9b9c-4107-8f96-97c85a945fbc.png)

* * * * * * * * * *

## Einleitung

Der **Q_NumericValue** ist ein standardkonformer Funktionsbaustein zur Änderung numerischer Werte in Virtual Terminals, entwickelt unter EPL-2.0 Lizenz. Die Version 1.0 implementiert die ISO 11783-6 (Teil 6 - F.22) Spezifikation für numerische VT-Objekte.

![Q_NumericValue](Q_NumericValue.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `INIT`: Initialisierungsanforderung (mit Objekt-ID)
- `REQ`: Wertänderungs-Anforderung

### **Ereignis-Ausgänge**

- `INITO`: Initialisierungsbestätigung
- `CNF`: Änderungsbestätigung

### **Daten-Eingänge**

- `u16ObjId` (UINT): Objekt-ID (16-bit)
- `u32NewValue` (UDINT): Neuer numerischer Wert (32-bit unsigned)

### **Daten-Ausgänge**

- `STATUS` (STRING): Betriebsstatusmeldung
- `u32OldValue` (UDINT): Vorheriger numerischer Wert
- `s16result` (INT): ISO-konformer Ergebniscode

## Gültige Objekt-IDs

**`u16ObjId` — gültige Objekttypen (Anhang F.22, Objekte mit numerischem Wert-Attribut):**
Input Boolean Field (7000–7999), Input Number Field (9000–9999), Input List Field (10000–10999), Output Number Field (12000–12999), Meter (17000–17999), Linear Bar Graph (18000–18999), Arched Bar Graph (19000–19999), Number Variable (21000–21999), Object Pointer (27000–27999), Output List Object (37000–37999), External Object Pointer (43000–43999), Animation Object (44000–44999), Scaled Graphic Object (48000–48999).

ID_NULL (65535) ist kein gültiges Kommandoziel, deaktiviert aber bei `INIT` den Baustein.

## Funktionsweise

1. **Initialisierung**:
   - `INIT` mit Zielobjekt-ID
   - `INITO` bestätigt Betriebsbereitschaft

2. **Wertaktualisierung**:
   - `REQ` mit neuem 32-Bit-Wert
   - Aktualisiert das numerische VT-Objekt
   - `CNF` liefert Betriebsstatus und vorherigen Wert

3. **Wertbereich**:
   - 0 bis 4.294.967.295 (32-bit unsigned)

## Technische Besonderheiten

✔ **ISO 11783-6 konform** (F.22)
✔ **32-Bit-Wertebereich** (UDINT)
✔ **Sofortige Aktualisierung**
✔ **Rückverfolgbarkeit** (Vorheriger Wert)
✔ **Interne Pufferung**: Der Funktionsbaustein puffert den Wert intern. Eine Nachricht wird nur dann auf den Bus gesendet, wenn sich `u32NewValue` von `u32OldValue` unterscheidet. Dies reduziert die Buslast erheblich und verzeiht häufige REQ-Events.

## Wertebereich

| Parameter    | Typ       | Wertebereich          |
|-------------|-----------|-----------------------|
| u32NewValue | UDINT     | 0 bis 4.294.967.295   |

## Rückgabecodes (s16result)

| Code | Konstante               | Bedeutung                          |
|------|-------------------------|------------------------------------|
| 0    | VT_E_NO_ERR             | Erfolgreiche Änderung             |
| -6   | VT_E_OVERFLOW           | Pufferüberlauf                   |
| -8   | VT_E_NOACT              | VT nicht bereit                   |
| -21  | VT_E_NO_INSTANCE        | Kein VT-Client verfügbar          |
| -128 | VT_E_HANDLE_INVALID     | Ungültige Objekt-ID               |
| -129 | VT_E_ISO_INSTANCE_INVALID | Ungültige VT-Instanz             |
| -130 | VT_E_NOT_ALIVE          | VT nicht aktiv                    |

## Anwendungsszenarien

- **Prozessvisualisierung**: Echtzeit-Messwerte
- **Steuerungselemente**: Sollwertvorgaben
- **Diagnosesysteme**: Fehlercode-Anzeige
- **Produktionsdaten**: Zähler und Statistiken

## ⚖️ Vergleich mit ähnlichen Bausteinen

| Feature        | Q_NumericValue | VtNumberUpdate | VtDataManager |
|---------------|----------------|----------------|---------------|
| ISO-Standard  | ✔              | ✖              | ✖             |
| Wertebereich  | 32-bit         | 16-bit         | 32-bit        |
| Rückmeldung   | ✔              | ✖              | ✔             |
| Objekttyp     | Numerisch      | Alle           | Alle          |

## 🛠️ Zugehörige Übungen

- [Uebung_009](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_009/)
- [Uebung_009a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_009a/)
- [Uebung_011a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_011a/)
- [Uebung_011a2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_011a2/)
- [Uebung_012](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_012/)
- [Uebung_012a_sub](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_012a_sub/)
- [Uebung_012b](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_012b/)
- [Uebung_015](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_015/)
- [Uebung_015a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_015a/)
- [Uebung_020c2_sub](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_020c2_sub/)
- [Uebung_035](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_035/)
- [Uebung_035b](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_035b/)
- [Uebung_035c](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_035c/)
- [Uebung_036](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_036/)
- [Uebung_037](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_037/)
- [Uebung_038](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_038/)
- [Uebung_038_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_038_AX/)
- [Uebung_039_sub_NumbAnzeig](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_039_sub_NumbAnzeig/)
- [Uebung_040](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_040/)
- [Uebung_040_2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_040_2/)
- [Uebung_040_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_040_AX/)
- [Uebung_041](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_041/)
- [Uebung_070](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_070/)
- [Uebung_071](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_071/)
- [Uebung_071a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_071a/)
- [Uebung_071b](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_071b/)
- [Uebung_072](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_072/)
- [Uebung_072b](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_072b/)
- [Uebung_072c](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_072c/)
- [Uebung_073](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_073/)
- [Uebung_074](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_074/)
- [Uebung_083](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_083/)

## Fazit

Der Q_NumericValue-Baustein bietet präzise numerische Steuerung:

- **Hochauflösend**: 32-Bit-Präzision
- **Zuverlässig**: Integrierte Fehlerprüfung
- **Flexibel**: Für alle numerischen Objekte

Essential für:

- Präzise Prozessvisualisierung
- Echtzeit-Datenmonitoring
- Industrielle Steuerungssysteme

## Beispielanwendungen

[Q_NumericValue_beispiele](Q_NumericValue_beispiele.md)

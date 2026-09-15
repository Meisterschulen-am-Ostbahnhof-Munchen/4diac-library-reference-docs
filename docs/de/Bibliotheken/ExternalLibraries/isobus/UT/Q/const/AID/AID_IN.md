# AID_IN

![AID_IN](./AID_IN.svg)

* * * * * * * * * *

## Einleitung

Der GlobalConstants-Baustein `AID_IN` definiert die Attribut-IDs für ein **Input Number Objekt** im ISOBUS‑Standard (ISO 11783-6).

## Schnittstellenstruktur

### **Ereignis-Eingänge**
Keine.

### **Ereignis-Ausgänge**
Keine.

### **Daten-Eingänge**
Keine.

### **Daten-Ausgänge**
Keine.

### **Adapter**
Keine.

## Funktionsweise

Gemäß ISO 11783-6 Tabelle B.18 kennzeichnen eckige Klammern `[ ]` um eine Attribut-ID (wie `[14]` `VALUE` und `[15]` `OPTIONS_2`) ein **Nur-Lese-Attribut (read-only)** für das *Change Attribute* Kommando, abfragbar via *Get Attribute Value* (F.58):

| Konstante | Datentyp | Wert | Beschreibung |
|---|---|---|---|
| `WIDTH` | `USINT` | `1` | Breite in Pixeln (beschreibbar via *Change Attribute* F.38). |
| `HEIGHT` | `USINT` | `2` | Höhe in Pixeln (beschreibbar via *Change Attribute* F.38). |
| `BACKGROUND_COLOUR` | `USINT` | `3` | Index der Hintergrundfarbe (beschreibbar via *Change Attribute* F.38). |
| `FONT_ATT` | `USINT` | `4` | Objekt-ID eines Font Attributes Objekts (beschreibbar via *Change Attribute* F.38). |
| `OPTIONS` | `USINT` | `5` | Options-Bitmaske (beschreibbar via *Change Attribute* F.38). |
| `VARIABLE_REF` | `USINT` | `6` | Objekt-ID eines Variable Objekts (beschreibbar via *Change Attribute* F.38). |
| `MIN_VALUE` | `USINT` | `7` | Minimalwert (beschreibbar via *Change Attribute* F.38). |
| `MAX_VALUE` | `USINT` | `8` | Maximalwert (beschreibbar via *Change Attribute* F.38). |
| `OFFSET` | `USINT` | `9` | Offset (beschreibbar via *Change Attribute* F.38). |
| `SCALE` | `USINT` | `10` | Skalierungsfaktor (beschreibbar via *Change Attribute* F.38). |
| `NUMB_DECIMALS` | `USINT` | `11` | Anzahl Nachkommastellen (beschreibbar via *Change Attribute* F.38). |
| `FORMAT` | `USINT` | `12` | Anzeigeformat (beschreibbar via *Change Attribute* F.38). |
| `JUSTIFICATION` | `USINT` | `13` | Ausrichtung (beschreibbar via *Change Attribute* F.38). |
| `VALUE` | `USINT` | `[14]` | **Nur-Lesen (Read-only)** für *Change Attribute* (ISO 11783-6). Unskalierter Rohwert. Abfragbar via *Get Attribute Value* (F.58); Änderung zur Laufzeit via *Change Numeric Value* (F.22). |
| `OPTIONS_2` | `USINT` | `[15]` | **Nur-Lesen (Read-only)** für *Change Attribute* (ISO 11783-6). Bitmaske (Bit 0 = Enabled, Bit 1 = Real time editing). Abfragbar via *Get Attribute Value* (F.58); Steuerung via *Select Input Object* (F.6). |

## Anwendungsszenarien

- Konfiguration der Eigenschaften eines Input Number Objekts via *Change Attribute* (F.38).
- Abfragen des unskalierten Rohwerts (`AID_IN.VALUE` `[14]`) via *Get Attribute Value* (F.58) oder Änderung zur Laufzeit via *Change Numeric Value* (F.22).
- Fokus- und Aktivierungssteuerung (`AID_IN.OPTIONS_2` `[15]`) via *Select Input Object* (F.6).

## Fazit

`AID_IN` stellt eine zentrale, standardkonforme Schnittstelle für die Attribut-IDs von Input Number Objekten bereit.

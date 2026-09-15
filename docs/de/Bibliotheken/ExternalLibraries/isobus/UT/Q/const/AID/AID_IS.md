# AID_IS

![AID_IS](./AID_IS.svg)

* * * * * * * * * *

## Einleitung

Der GlobalConstants-Baustein `AID_IS` definiert die Attribut-IDs für ein **Input String Objekt** im ISOBUS‑Standard (ISO 11783-6). Diese Konstanten ermöglichen den standardisierten Zugriff auf Eigenschaften eines Input String Objekts.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
Keine.

### **Ereignis-Ausgänge**
Keine.

### **Daten-Eingänge**

Gemäß ISO 11783-6 Tabelle B.17 kennzeichnen eckige Klammern `[ ]` um eine Attribut-ID (wie `[9]` `ENABLED`) ein **Nur-Lese-Attribut (read-only)** für das *Change Attribute* Kommando, abfragbar via *Get Attribute Value* (F.58):

| Name | Typ | Wert | Beschreibung |
|---|---|---|---|
| `WIDTH` | `USINT` | `1` | Breite in Pixeln (beschreibbar via *Change Attribute* F.38). |
| `HEIGHT` | `USINT` | `2` | Höhe in Pixeln (beschreibbar via *Change Attribute* F.38). |
| `BACKGROUND_COLOUR` | `USINT` | `3` | Index der Hintergrundfarbe (beschreibbar via *Change Attribute* F.38). |
| `FONT_ATT` | `USINT` | `4` | Objekt-ID eines Font Attributes Objekts (beschreibbar via *Change Attribute* F.38). |
| `INP_ATT` | `USINT` | `5` | Objekt-ID eines Input Attributes / Extended Input Attributes Objekts (beschreibbar via *Change Attribute* F.38). |
| `OPTIONS` | `USINT` | `6` | Options-Bitmaske (beschreibbar via *Change Attribute* F.38). |
| `VARIABLE_REF` | `USINT` | `7` | Objekt-ID eines String Variable Objekts (beschreibbar via *Change Attribute* F.38). |
| `JUSTIFICATION` | `USINT` | `8` | Textausrichtung: Bits 0-1 (Horizontal), Bits 2-3 (Vertikal) (beschreibbar via *Change Attribute* F.38). |
| `ENABLED` | `USINT` | `[9]` | **Nur-Lesen (Read-only)** für *Change Attribute* (ISO 11783-6). 0 = deaktiviert, 1 = aktiviert. Abfragbar via *Get Attribute Value* (F.58); Steuerung via *Select Input Object* (F.6). |

## Anwendungsszenarien

- Konfiguration der Abmessungen und visuellen Eigenschaften eines Input String Objekts via *Change Attribute* (F.38).
- Abfragen des Aktivierungsstatus (`AID_IS.ENABLED` `[9]`) via *Get Attribute Value* (F.58) oder Fokussteuerung via *Select Input Object* (F.6).

## Fazit

`AID_IS` stellt eine saubere, typensichere Schnittstelle für die Attribut-IDs von Input String Objekten bereit.

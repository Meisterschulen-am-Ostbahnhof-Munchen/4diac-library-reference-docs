# AID_IL

![AID_IL](./AID_IL.svg)

* * * * * * * * * *

## Einleitung

`AID_IL` ist eine globale Konstantengruppe für ISOBUS-Anwendungen. Sie definiert die Attribut-IDs für ein **Input List** Objekt eines ISOBUS Universal Terminals (UT) gemäß ISO 11783-6. Die Konstantengruppe ist kein Funktionsbaustein oder Adapter; sie stellt eine benannte, wiederverwendbare Sammlung numerischer Attribut-IDs bereit.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
Keine.

### **Ereignis-Ausgänge**
Keine.

### **Daten-Eingänge**
Keine.

### **Daten-Ausgänge**
Keine (Zugriff global via `AID_IL.<NAME>`).

### **Adapter**
Keine.

## Funktionsweise

`AID_IL` bündelt die Attribut-IDs eines ISOBUS UT **Input List** Objekts. Gemäß ISO 11783-6 Tabelle B.20 kennzeichnen eckige Klammern `[ ]` um eine Attribut-ID (wie `[4]` `VALUE` und `[5]` `OPTIONS`) ein **Nur-Lese-Attribut (read-only)** für das *Change Attribute* Kommando, abfragbar via *Get Attribute Value* (F.58):

| Konstante | Datentyp | Wert | Beschreibung |
|---|---|---|---|
| `WIDTH` | `USINT` | `1` | `AID_IL_WIDTH` – Breite der Eingabeliste in Pixeln (beschreibbar via *Change Attribute* F.38). |
| `HEIGHT` | `USINT` | `2` | `AID_IL_HEIGHT` – Höhe der Eingabeliste in Pixeln (beschreibbar via *Change Attribute* F.38). |
| `VARIABLE_REF` | `USINT` | `3` | `AID_IL_VARIABLE_REF` – Objekt-ID eines Number Variable Objekts (beschreibbar via *Change Attribute* F.38). |
| `VALUE` | `USINT` | `[4]` | `AID_IL_VALUE` – **Nur-Lesen (Read-only)** für *Change Attribute* (ISO 11783-6). Ausgewählter Listenindex (0–254, 255=kein Element). Abfragbar via *Get Attribute Value* (F.58); Änderung zur Laufzeit via *Change Numeric Value* (F.22). |
| `OPTIONS` | `USINT` | `[5]` | `AID_IL_OPTIONS` – **Nur-Lesen (Read-only)** für *Change Attribute* (ISO 11783-6). Bitmaske für Optionen (Bit 0 = Enabled, Bit 1 = Real time editing). Abfragbar via *Get Attribute Value* (F.58); Steuerung via *Enable/Disable Object* (F.4) oder *Select Input Object* (F.6). |

## Anwendungsszenarien

`AID_IL` wird eingesetzt, wenn Attribut-IDs eines Input List Objekts abgefragt oder gesetzt werden:
- Einstellung von Breite und Höhe des Objekts via *Change Attribute* (F.38).
- Verknüpfung mit einem Number Variable Objekt.
- Abfrage des aktuell gewählten Listenindex (`AID_IL.VALUE` `[4]`) via *Get Attribute Value* (F.58) oder Änderung via *Change Numeric Value* (F.22).
- Steuerung des Aktivierungsstatus (`AID_IL.OPTIONS` `[5]`) via *Select Input Object* (F.6).

## Fazit

`AID_IL` bietet eine klare, standardkonforme Möglichkeit, Attribut-IDs für Input List Objekte in ISOBUS-Terminals zu adressieren.

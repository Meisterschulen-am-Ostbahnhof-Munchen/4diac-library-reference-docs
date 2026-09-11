# AID_OS

![AID_OS](./AID_OS.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `AID_OS` stellt eine Sammlung globaler Konstanten zur Verfügung, die als Attribut-IDs (AID) für **Output String Objekte** im ISOBUS-Kontext (ISO 11783) dienen. Diese Konstanten werden verwendet, um auf die verschiedenen Attribute eines Output‑String‑Objekts in der landwirtschaftlichen Elektronik zuzugreifen. Sie sind als `GlobalConstants` in IEC 61499 definiert und erlauben eine einheitliche, symbolische Referenzierung anstelle von magischen Zahlenwerten.

## Schnittstellenstruktur

Da `AID_OS` keine klassischen Ein-/Ausgänge besitzt, stellt es ausschließlich **globale Konstanten** bereit. Diese können im gesamten Projekt verwendet werden und sind in der folgenden Tabelle aufgeführt:

| Konstantenname | Typ | Wert | Bedeutung |
|----------------|-----|------|-----------|
| `WIDTH` | USINT | 1 | Breite des Objekts in Pixeln (AID_OS_WIDTH) |
| `HEIGHT` | USINT | 2 | Höhe des Objekts in Pixeln (AID_OS_HEIGHT) |
| `BACKGROUND_COLOUR` | USINT | 3 | Farbindex des Hintergrunds |
| `FONT_ATT` | USINT | 4 | Objekt‑ID eines Font‑Attributes‑Objekts |
| `OPTIONS` | USINT | 5 | Bitmaske: Bit 0 = transparent, Bit 1 = automatischer Umbruch, Bit 2 = Umbruch am Bindestrich |
| `VARIABLE_REF` | USINT | 6 | Objekt‑ID eines Variablen‑Objekts |
| `JUSTIFICATION` | USINT | 7 | Ausrichtung: Bits 0‑1 (horizontal: 0=links, 1=mittig, 2=rechts), Bits 2‑3 (vertikal: 0=oben, 1=mittig, 2=unten) |

### **Ereignis-Eingänge**

Keine vorhanden.

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

Keine vorhanden.

### **Daten-Ausgänge**

Keine direkten Ausgänge – die Konstanten sind global verfügbar und können über den qualifizierten Namen (z.B. `AID_OS.WIDTH`) angesprochen werden.

### **Adapter**

Keine vorhanden.

## Funktionsweise

Der Baustein definiert sieben Konstanten, die die numerischen IDs der Attribute eines Output‑String‑Objekts im ISOBUS‑Protokoll repräsentieren. Diese IDs werden bei der Konfiguration von Terminal‑Objekten in der Landmaschinen‑Elektronik verwendet, um z.B. die Darstellung und das Verhalten von Textausgaben festzulegen. Durch die Verwendung symbolischer Namen wird der Code lesbarer und unabhängig von konkreten Zahlenwerten. Die Konstanten sind als `USINT` (Unsigned Short Integer) mit festen Initialwerten definiert und im globalen Namensraum verfügbar.

## Technische Besonderheiten

- **Typ**: Alle Werte sind `USINT` (8‑Bit, vorzeichenlos).
- **Bitmanipulation**: Die Konstanten `OPTIONS` und `JUSTIFICATION` enthalten bitweise kodierte Informationen. Die Bedeutung der einzelnen Bits ist in den Kommentaren dokumentiert.
- **ISOBUS‑Konformität**: Die Attribut‑IDs entsprechen den Spezifikationen des ISOBUS‑Standards (ISO 11783) für Output‑String‑Objekte.
- **Einsatz**: Diese Konstanten werden typischerweise zusammen mit den ISOBUS‑Funktionsbausteinen (z.B. für Objektmanagement) verwendet.

## Zustandsübersicht

Da es sich um reine Konstanten handelt, existiert kein Verhalten oder Zustandsautomat. Der Baustein ist statisch und besitzt keine internen Zustände.

## Anwendungsszenarien

Ein typisches Anwendungsbeispiel ist die Erstellung eines Output‑String‑Objekts in einem ISOBUS‑Terminal. Hierbei müssen die Attribute des Objekts (Breite, Höhe, Hintergrundfarbe, Schriftart usw.) über deren AID‑Werte gesetzt werden. Durch die Verwendung von `AID_OS.WIDTH`, `AID_OS.HEIGHT` usw. wird der Code selbstdokumentierend und leicht an Änderungen im Standard anpassbar.

Beispiel (ST‑Syntax):

```
myOutputString.Width := AID_OS.WIDTH;
myOutputString.Options := AID_OS.OPTIONS; // transparent, auto-wrap
```

## Vergleich mit ähnlichen Bausteinen

Im selben Kontext existieren weitere GlobalConstants‑Bausteine für andere Objekttypen, z.B. `AID_OS` für Output‑Strings, `AID_OT` für Objekt‑Typen oder `AID_VAR` für Variablen. Diese folgen dem gleichen Muster und stellen die jeweiligen Attribut‑IDs bereit. `AID_OS` ist speziell auf Output‑String‑Objekte zugeschnitten.

## Fazit

`AID_OS` ist ein einfacher, aber nützlicher Konstantencontainer, der die Entwicklung von ISOBUS‑kompatiblen Anwendungen vereinfacht. Durch die zentrale Definition der Attribut‑IDs wird die Wartbarkeit erhöht und Fehler durch falsche Zahlenwerte werden vermieden. Der Baustein ist statisch und robust und eignet sich ideal für den Einsatz in allen Projekten, die mit ISOBUS‑Terminals kommunizieren.

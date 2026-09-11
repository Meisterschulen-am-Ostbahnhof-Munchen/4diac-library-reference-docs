# AID_EXOP

![AID_EXOP](./AID_EXOP.svg)

* * * * * * * * * *
## Einleitung

Der globale Konstantenblock `AID_EXOP` definiert Attribut-IDs für das ISOBUS‑Objektmodell zur Identifikation von Attributen des „External Object Pointer“. Diese IDs werden verwendet, um auf externe Objektverweise, deren Namen und Standarddarstellungen zuzugreifen. Der Block ist Teil des Pakets `isobus::UT::Q::const::AID` und stellt einheitliche Konstanten für die Kommunikation in landwirtschaftlichen Maschinen gemäß ISO 11783 dar.

## Schnittstellenstruktur

Dieser Block besitzt weder Ereignis‑ noch Datenein‑ oder -ausgänge, da er ausschließlich als **globale Konstantenquelle** dient. Alle Werte sind zur Laufzeit unveränderlich.

### **Ereignis-Eingänge**
Nicht vorhanden.

### **Ereignis-Ausgänge**
Nicht vorhanden.

### **Daten-Eingänge**
Nicht vorhanden.

### **Daten-Ausgänge**
Nicht vorhanden.

### **Adapter**
Nicht vorhanden.

**Bereitgestellte Konstanten** (wertvoll für die Verwendung in jeglicher Logik):

| Konstante          | Typ    | Wert | Beschreibung |
|--------------------|--------|------|--------------|
| `DEF_OBJ_ID`       | USINT  | 1    | Standard‑Objekt‑ID, die angezeigt wird, wenn eine externe ID ungültig oder `NULL` ist. |
| `EXT_REF_NAME_ID`  | USINT  | 2    | Attribut‑ID für den Namen des externen Objektverweises. |
| `EXT_OBJ`          | USINT  | 3    | Attribut‑ID für das externe Objekt selbst. |

## Funktionsweise

Die drei Konstanten repräsentieren Attributkennungen (AIDs) für den „External Object Pointer“ (EXOP) im ISOBUS‑Datenmodell. Sie werden typischerweise als Parameter bei der Abfrage oder Manipulation von Objektattributen über den Objekt‑Pool‑Zugriff verwendet. Durch die Definition als globale Konstanten wird eine einheitliche und fehlerfreie Referenzierung über mehrere Funktionsblöcke hinweg ermöglicht.

## Technische Besonderheiten

- **Datentyp:** Alle Konstanten sind vom Typ `USINT` (Unsigned Short Integer, 8 Bit).
- **Wertebereich:** Die Werte 1–3 entsprechen den standardisierten Attribut‑IDs laut ISOBUS‑Spezifikation.
- **Eigentum:** Der Block ist unter der Eclipse Public License 2.0 (EPL‑2.0) veröffentlicht und wurde von der HR Agrartechnik GmbH entwickelt.
- **Compiler‑Paket:** `isobus::UT::Q::const::AID` – die Konstanten werden in den generierten C++‑Code eingebettet und sind dort als `const` verfügbar.

## Zustandsübersicht

Da es sich um einen Konstantenblock handelt, existiert kein dynamischer Zustand oder eine Zustandsmaschine. Die Werte sind während der gesamten Laufzeit stabil und unveränderlich.

## Anwendungsszenarien

- **ISOBUS‑Implementierung:** Verwendung in Funktionsblöcken, die auf externe Objektverweise innerhalb eines ISOBUS‑Terminals zugreifen müssen.
- **Objekt‑Pool‑Steuerung:** Referenzierung der IDs bei der Definition von Objekt‑Attributen in einem Virtuellen Terminal (VT).
- **Fehlerbehandlung:** Einsatz der Konstanten zur Bestimmung einer Standard‑Darstellung, falls eine externe Objekt‑ID nicht aufgelöst werden kann.

## Vergleich mit ähnlichen Bausteinen

Innerhalb desselben Pakets (`isobus::UT::Q::const::AID`) können weitere Konstantenblöcke existieren, die andere Attribut‑IDs (z.B. für das Arbeitsmengen‑Management oder den Task‑Controller) definieren. `AID_EXOP` ist spezifisch auf den „External Object Pointer“ zugeschnitten und stellt nur die dafür relevanten AIDs bereit. Andere Blöcke haben ähnliche Struktur, unterscheiden sich jedoch in den enthaltenen Konstanten.

## Fazit

Der globale Konstantenblock `AID_EXOP` bietet eine klar definierte und zentrale Sammlung von Attribut‑IDs für den ISOBUS‑Mechanismus der externen Objektverweise. Er vereinfacht die Implementierung, erhöht die Lesbarkeit des Codes und reduziert Fehlerquellen durch hartcodierte Zahlen. Durch die Verwendung als globale Konstante wird die Wartbarkeit und Wiederverwendbarkeit von ISOBUS‑Anwendungen in der 4diac‑Umgebung verbessert.
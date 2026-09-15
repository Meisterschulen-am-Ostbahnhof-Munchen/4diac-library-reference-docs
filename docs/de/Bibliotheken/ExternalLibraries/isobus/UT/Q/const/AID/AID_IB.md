# AID_IB

![AID_IB](./AID_IB.svg)

* * * * * * * * * *

## Einleitung

Der GlobalConstants-Baustein `AID_IB` definiert die Attribut-IDs für ein **Input Boolean Objekt** im ISOBUS‑Standard (ISO 11783-6). Diese Konstanten werden verwendet, um auf einzelne Attribute eines solchen Objekts in einem ISOBUS‑Terminal zuzugreifen, z.⯯. für Hintergrundfarbe, Breite, Wert oder Aktivierungsstatus. Der Baustein ist Teil des Pakets `isobus::UT::Q::const::AID` und stellt eine zentrale Sammlung numerischer Identifikatoren zur Verfügung, die in der Anwendungslogik und in Protokollnachrichten referenziert werden.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine (definiert als GlobalConstants).

### **Ereignis-Ausgänge**

Keine (definiert als GlobalConstants).

### **Daten-Eingänge**

Keine – der Baustein stellt ausschließlich Konstanten bereit, keine Eingangsparameter.

### **Daten-Ausgänge**

Keine – der Baustein ist nicht als ausführbarer Funktionsblock konzipiert.

### **Adapter**

Keine – es werden keine Adapter bereitgestellt.

### **Globale Konstanten**

Gemäß ISO 11783-6 kennzeichnen eckige Klammern `[ ]` um eine Attribut-ID (wie `[5]` `VALUE` und `[6]` `ENABLED`) ein **Nur-Lese-Attribut (read-only)** für das *Change Attribute* Kommando. Diese Attribute sind über die *Get Attribute Value* Nachricht (F.58) abfragbar:

| Konstante | Datentyp | Wert | Beschreibung |
|---|---|---|---|
| `BACKGROUND_COLOUR` | `USINT` | `1` | Index der Hintergrundfarbe des Objekts (beschreibbar via *Change Attribute* F.38). |
| `WIDTH` | `USINT` | `2` | Breite des Objekts in Pixeln (beschreibbar via *Change Attribute* F.38). |
| `FG_COLOUR` | `USINT` | `3` | Objekt‑ID eines Font‑Attributes‑Objekts zur Bestimmung der Schriftfarbe (beschreibbar via *Change Attribute* F.38). |
| `VARIABLE_REF` | `USINT` | `4` | Objekt‑ID eines Number‑Variable‑Objekts; wenn `NULL`, wird der Wert direkt gespeichert (beschreibbar via *Change Attribute* F.38). |
| `VALUE` | `USINT` | `[5]` | **Nur-Lesen (Read-only)** für *Change Attribute* (ISO 11783-6). Aktueller Wert des Eingabefelds (`0` = FALSE, `>0` = TRUE). Abfragbar via *Get Attribute Value* (F.58); Änderung zur Laufzeit via speziellem *Change Numeric Value* Kommando (F.22). |
| `ENABLED` | `USINT` | `[6]` | **Nur-Lesen (Read-only)** für *Change Attribute* (ISO 11783-6). Aktivierungsstatus (`0` = deaktiviert, `1` = aktiviert). Abfragbar via *Get Attribute Value* (F.58); Steuerung via *Enable/Disable Object* (F.4) oder *Select Input Object* (F.6). |

## Funktionsweise

Die Konstanten von `AID_IB` kennzeichnen die numerischen Attribut‑IDs eines **Input Boolean Objekts** im ISOBUS‑Protokoll. Jede ID entspricht einem spezifischen Attribut, das in Nachrichten (z.⯯. Get/Set‑Kommandos) verwendet wird, um Werte auszulesen oder zu setzen. Die Werte sind Teil des standardisierten Objektmodells für Virtual Terminals und ermöglichen eine einheitliche Adressierung der Objekteigenschaften. Durch die Definition als globale Konstanten können Referenzen im Quellcode zentral und wartungsfreundlich verwendet werden.

## Technische Besonderheiten

- **Lizenz & Copyright:** Der Baustein ist unter der **Eclipse Public License 2.0** veröffentlicht (SPDX‑Identifier: EPL‑2.0).  
- **Versionierung:** Version `1.0`, Stand `2026-06-20`, erstellt von Franz Höpfinger (HR Agrartechnik GmbH).  
- **Compilerhinweis:** Der Quellcode ist für das Zielpaket `isobus::UT::Q::const::AID` vorgesehen.  
- **Konstantentyp:** Alle Werte sind vom Typ `USINT` (Unsigned Short Integer, 8 Bit).  
- **Struktur:** Der Baustein enthält keine ausführbare Logik – er dient ausschließlich der Bereitstellung von Konstanten.

## Zustandsübersicht

Da es sich um einen GlobalConstants‑Baustein handelt, existiert **kein Zustandsmodell**. Er besitzt weder interne Zustände noch eine Zustandsmaschine.

## Anwendungsszenarien

`AID_IB` wird typischerweise in Projekten eingesetzt, die **ISOBUS‑Terminals** (Virtual Terminals) steuern. Anwendungsbeispiele:  

- **Objektpool-Generierung:** Bei der Erstellung eines VT-Objektpools dienen diese Konstanten als Attribut-Selektoren.
- **Attribut-Abfrage via `GetAttribute` (F.58):** Anwendungen, die den Zustand oder den Wert eines Input Boolean Objekts abfragen wollen, nutzen `AID_IB.VALUE` (`[5]`) oder `AID_IB.ENABLED` (`[6]`) in *Get Attribute Value* Anfragen.
- **Laufzeit-Wertänderung via `Change Numeric Value` (F.22):** Zur dynamischen Änderung des Schalterwerts wird das spezialisierte Kommando *Change Numeric Value* (F.22) gesendet, da `VALUE` (`[5]`) für *Change Attribute* als read-only eingestuft ist.
- **Optische Anpassung via `Change Attribute` (F.38):** Dynamische Modifikationen visueller Attribute (`BACKGROUND_COLOUR`, `WIDTH`, `FG_COLOUR`, `VARIABLE_REF`) nutzen das allgemeine *Change Attribute* Kommando.

## Vergleich mit ähnlichen Bausteinen

Für andere GUI‑Objekte existieren analoge Konstantensätze, z.⯯. `AID_KEY` oder `AID_OUTPUT_STRING`. Der Unterschied liegt in den spezifischen Attributen:  

- **Input Boolean** besitzt Attribute wie `VALUE` (boolesch) und `ENABLED`.  
- **Output String** würde z.⯯. `TEXT`, `FONT_ATTRIBUTE` oder `TEXT_ALIGNMENT` enthalten.  
Der `AID_IB`‑Baustein ist speziell auf die Bedürfnisse von booleschen Eingabefeldern zugeschnitten, während andere Bausteine andere Attributsätze abdecken.

## Fazit

Der GlobalConstants‑Baustein `AID_IB` ist eine essenzielle Sammlung von Attribut‑IDs für ISOBUS‑Eingabefelder vom Typ Boolean. Er vereinfacht die Entwicklung von Anwendungen, indem er die numerischen Kennungen zentral und klar dokumentiert bereitstellt. Die Verwendung der Konstanten erhöht die Lesbarkeit des Codes und minimiert Fehler bei der Adressierung von Objektattributen. Durch die standardkonforme Lizenzierung und Versionierung ist er für industrielle Anwendungen geeignet.

# AID_IB

![AID_IB](./AID_IB.svg)

* * * * * * * * * *
## Einleitung
Der GlobalConstants-Baustein `AID_IB` definiert die Attribut-IDs für ein **Input Boolean Objekt** im ISOBUS‑Standard (ISO 11783). Diese Konstanten werden verwendet, um auf einzelne Attribute eines solchen Objekts in einem ISOBUS‑Terminal zuzugreifen, z. B. für Hintergrundfarbe, Breite, Wert oder Aktivierungsstatus. Der Baustein ist Teil des Pakets `isobus::UT::Q::const::AID` und stellt eine zentrale Sammlung numerischer Identifikatoren zur Verfügung, die in der Anwendungslogik und in Protokollnachrichten referenziert werden.

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
| Konstante           | Datentyp | Wert | Beschreibung |
|---------------------|----------|------|--------------|
| `BACKGROUND_COLOUR` | `USINT`  | `1`  | Index der Hintergrundfarbe des Objekts. |
| `WIDTH`             | `USINT`  | `2`  | Breite des Objekts in Pixeln. |
| `FG_COLOUR`         | `USINT`  | `3`  | Objekt‑ID eines Font‑Attributes‑Objekts zur Bestimmung der Schriftfarbe. |
| `VARIABLE_REF`      | `USINT`  | `4`  | Objekt‑ID eines Number‑Variable‑Objekts; wenn `NULL`, wird der Wert direkt gespeichert. |
| `VALUE`             | `USINT`  | `5`  | Aktueller Wert des Eingabefelds: `0` = FALSE, `>0` = TRUE. |
| `ENABLED`           | `USINT`  | `6`  | Aktivierungsstatus: `0` = deaktiviert, `1` = aktiviert. |

## Funktionsweise
Die Konstanten von `AID_IB` kennzeichnen die numerischen Attribut‑IDs eines **Input Boolean Objekts** im ISOBUS‑Protokoll. Jede ID entspricht einem spezifischen Attribut, das in Nachrichten (z. B. Get/Set‑Kommandos) verwendet wird, um Werte auszulesen oder zu setzen. Die Werte sind Teil des standardisierten Objektmodells für Virtual Terminals und ermöglichen eine einheitliche Adressierung der Objekteigenschaften. Durch die Definition als globale Konstanten können Referenzen im Quellcode zentral und wartungsfreundlich verwendet werden.

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
- Konfiguration eines **Input‑Boolean‑Objekts** (z. B. Ein‑/Ausschalter) auf der Benutzeroberfläche.  
- Abrufen oder Setzen von Attributen wie `VALUE` oder `ENABLED` zur Anzeige und Bedienung von Schaltern.  
- Kombination mit Funktionsbausteinen, die auf die Konstanten zugreifen, um die Kommunikation mit dem Terminal zu vereinheitlichen.  

## Vergleich mit ähnlichen Bausteinen
Für andere GUI‑Objekte existieren analoge Konstantensätze, z. B. `AID_KEY` oder `AID_OUTPUT_STRING`. Der Unterschied liegt in den spezifischen Attributen:  
- **Input Boolean** besitzt Attribute wie `VALUE` (boolesch) und `ENABLED`.  
- **Output String** würde z. B. `TEXT`, `FONT_ATTRIBUTE` oder `TEXT_ALIGNMENT` enthalten.  
Der `AID_IB`‑Baustein ist speziell auf die Bedürfnisse von booleschen Eingabefeldern zugeschnitten, während andere Bausteine andere Attributsätze abdecken.

## Fazit
Der GlobalConstants‑Baustein `AID_IB` ist eine essenzielle Sammlung von Attribut‑IDs für ISOBUS‑Eingabefelder vom Typ Boolean. Er vereinfacht die Entwicklung von Anwendungen, indem er die numerischen Kennungen zentral und klar dokumentiert bereitstellt. Die Verwendung der Konstanten erhöht die Lesbarkeit des Codes und minimiert Fehler bei der Adressierung von Objektattributen. Durch die standardkonforme Lizenzierung und Versionierung ist er für industrielle Anwendungen geeignet.
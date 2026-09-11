# IG1_NAME_Functions

![IG1_NAME_Functions](./IG1_NAME_Functions.svg)

* * * * * * * * * *

## Einleitung

Die GlobalConstants-Definition **IG1_NAME_Functions** stellt Konstanten für die Identifikation von Funktionen in Fahrzeugsystemen nach ISO 11783-6 (ISOBUS) bereit. Sie ist Teil der 4diac-IDE und wird als Baustein zur Bereitstellung dieser Konstanten verwendet. Die Werte sind als **BYTE**-Typ definiert und decken verschiedene Funktionsbereiche für unterschiedliche Fahrzeugsysteme (allgemeine Systeme, Traktor, Anhänger) ab.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine vorhanden.

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

Keine vorhanden.

### **Daten-Ausgänge**

Die Konstanten werden als Daten-Ausgänge betrachtet, da sie einen festen, unveränderlichen Wert bereitstellen. Die folgende Tabelle listet alle definierten Konstanten auf:

| Name | Typ | Wert (dezimal) | Kommentar |
|------|-----|----------------|-----------|
| **Non-specific System (System 0)** | | | |
| `F_TACHOGRAPH` | BYTE | 128 | Nicht-spezifisches System, Funktion: Tachograph |
| `F_DOOR_CONTROLLER` | BYTE | 129 | Nicht-spezifisches System, Funktion: Türsteuerung |
| `F_ARTICULATION_TURNTABLE_CONTROL` | BYTE | 130 | Nicht-spezifisches System, Funktion: Gelenkdrehkranzsteuerung (für Gelenkbusse) |
| `F_BODY_TO_VEHICLE_INTERFACE_CONTROL` | BYTE | 131 | Nicht-spezifisches System, Funktion: Schnittstellensteuerung zwischen Fahrzeug und Aufbau |
| `F_SLOPE_SENSOR` | BYTE | 132 | Nicht-spezifisches System, Funktion: Hangneigungssensor |
| `F_RETARDER_DISPLAY` | BYTE | 134 | Nicht-spezifisches System, Funktion: Anzeige des Retarders (Antriebsstrang, Abgas, Motor) |
| `F_DIFFERENTIAL_LOCK_CONTROLLER` | BYTE | 135 | Nicht-spezifisches System, Funktion: Differentialsperre-Steuerung |
| `F_LOW_VOLTAGE_DISCONNECT` | BYTE | 136 | Nicht-spezifisches System, Funktion: Niederspannungs-Trenneinrichtung (Batteriewächter) |
| `F_ROADWAY_INFORMATION` | BYTE | 137 | Nicht-spezifisches System, Funktion: Straßeninformation |
| `F_AUTOMATED_DRIVING` | BYTE | 138 | Nicht-spezifisches System, Funktion: Automatisiertes Fahren |
| `F_NOT_AVAILABLE` | BYTE | 255 | Nicht-spezifisches System, Funktion: Nicht verfügbar (bis eine Zuordnung erfolgt) |
| **Tractor (System 1)** | | | |
| `F_TRACTOR_FORWARD_ROAD_IMAGE_PROCESSING` | BYTE | 128 | Traktor, Funktion: Bilderfassung der Fahrbahn (Spurmarkierungserkennung) |
| `F_TRACTOR_FIFTH_WHEEL_SMART_SYSTEM` | BYTE | 129 | Traktor, Funktion: Intelligentes Sattelkupplungssystem (Betrieb und Sicherheitsüberwachung) |
| `F_TRACTOR_CATALYST_FLUID_SENSOR` | BYTE | 130 | Traktor, Funktion: Katalysator-Flüssigkeitssensor (Temperatur, Füllstand, Qualität) |
| `F_TRACTOR_ADAPTIVE_FRONT_LIGHTING_SYSTEM` | BYTE | 131 | Traktor, Funktion: Adaptives Frontbeleuchtungssystem (Stadt, Land, Autobahn) |
| `F_TRACTOR_IDLE_CONTROL_SYSTEM` | BYTE | 132 | Traktor, Funktion: Leerlaufsteuerung (automatisches Start/Stopp im Stillstand) |
| `F_TRACTOR_USER_INTERFACE_SYSTEM` | BYTE | 133 | Traktor, Funktion: Benutzerschnittstelle (bidirektional, z. B. Klima, Systemparameter) |
| `F_TRACTOR_NOT_AVAILABLE` | BYTE | 255 | Traktor, Funktion: Nicht verfügbar (bis eine Zuordnung erfolgt) |
| **Trailer (System 2)** | | | |
| `F_TRAILER_NOT_AVAILABLE` | BYTE | 255 | Anhänger, Funktion: Nicht verfügbar (bis eine Zuordnung erfolgt) |

### **Adapter**

Keine vorhanden.

## Funktionsweise

Die in `IG1_NAME_Functions` definierten Konstanten dienen dazu, eindeutige numerische Kennungen für Fahrzeugfunktionen innerhalb des ISOBUS-Kommunikationsprotokolls (ISO 11783) bereitzustellen. Jede Konstante entspricht einem bestimmten Funktionscode, der in Datenübertragungen (z. B. in PGNs – Parameter Group Numbers) verwendet wird, um Funktionen zu identifizieren. Die Aufteilung in die Gruppen (Non-specific, Tractor, Trailer) ermöglicht eine kollisionsfreie Zuordnung innerhalb der jeweiligen Systemkategorie. Der Wert 255 ist für den Fall vorgesehen, dass noch keine explizite Funktion zugewiesen wurde.

## Technische Besonderheiten

- Alle Konstanten sind vom Typ `BYTE` (Wertebereich 0–255).
- Die Werte 128–138 werden für spezifische Funktionen verwendet, wobei die Zuordnung innerhalb der Systemgruppen variiert (z. B. hat `F_TACHOGRAPH` und `F_TRACTOR_FORWARD_ROAD_IMAGE_PROCESSING` denselben Dezimalwert 128, jedoch unterschiedliche Systemkontexte).
- Die Gruppen (Non-specific, Tractor, Trailer) werden über eine separate Konstantendefinition (`DC_NON_SPECIFIC_SYSTEM`, `DC_TRACTOR`, `DC_TRAILER`) adressiert, die nicht Teil dieser Datei ist.
- Die Konstanten sind **global** und unveränderlich (CONSTANT), d. h. sie können während der Laufzeit nicht geändert werden.
- Die Werte sind als Initialwerte in der XML hinterlegt und können in 4diac-Projekten direkt referenziert werden.

## Zustandsübersicht

Da es sich um Konstanten handelt, existieren keine dynamischen Zustände oder Zustandsübergänge. Die Werte sind statisch und unveränderlich. Sie repräsentieren feste Funktionskennungen und unterliegen keiner Laufzeitlogik.

## Anwendungsszenarien

- **Automatische Konfiguration:** Steuergeräte im Fahrzeug können anhand der Funktionscodes ihre Rolle im ISOBUS-Netzwerk erkennen und sich entsprechend konfigurieren.
- **Diagnose und Wartung:** Die Konstanten erleichtern die Interpretation von Diagnosemeldungen, da jedem Code eine klare Bedeutung zugeordnet ist.
- **Entwicklung von ISOBUS-Applikationen:** Entwickler können diese Konstanten in der 4diac-IDE verwenden, um PGNs oder andere Protokollelemente zu parametrieren.
- **Erweiterte Funktionszuordnung:** Bei der Integration neuer Fahrzeugfunktionen können die vorhandenen Codes verwendet oder neue ergänzt werden, solange die Systemkategorie beachtet wird.

## Vergleich mit ähnlichen Bausteinen

Im ISOBUS-Umfeld existieren weitere GlobalConstants-Definitionen, z. B. für andere Industriegruppen (IG2, IG3 usw.) oder für allgemeine Fahrzeugparameter. Diese folgen einem ähnlichen Muster, unterscheiden sich jedoch in den konkreten Funktionscodes und Systemgruppen. Die vorliegende Definition ist speziell auf die Industriegruppe 1 (landwirtschaftliche Fahrzeuge und Anhänger) zugeschnitten. Sie bietet eine klare, wartbare Struktur, die eine einfache Erweiterung um weitere Funktionen ermöglicht.

## Fazit

**IG1_NAME_Functions** ist ein zentraler Konstantenbaustein für die ISOBUS-Entwicklung in der 4diac-IDE. Er stellt eindeutige Funktionskennungen für verschiedene Fahrzeugsysteme bereit und unterstützt so eine effiziente und standardkonforme Kommunikation. Die klare Trennung nach Systemgruppen sowie die Verwendung von sprechenden Namen machen den Baustein gut verständlich und ermöglichen eine einfache Integration in Steuerungs- und Diagnoseanwendungen.

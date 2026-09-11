# AID_SKM

![AID_SKM](./AID_SKM.svg)

* * * * * * * * * *
## Einleitung

Der Baustein **AID_SKM** ist ein globaler Konstanten-Container, der Attribut-Identifikatoren für Softkey-Masken-Objekte (Soft Key Mask Object Attribute IDs) im ISOBUS-Kontext definiert. Er wird im Paket `isobus::UT::Q::const::AID` bereitgestellt und dient als zentrale Referenz für einheitliche Werte, die in Steuerungsanwendungen zur Darstellung von Benutzeroberflächen verwendet werden.

## Schnittstellenstruktur

Da es sich um eine reine Konstantendeklaration handelt, besitzt der Baustein keine ein- oder ausgehenden Ereignisse, keine Daten-Ein-/Ausgänge und keine Adapter.

### **Ereignis-Eingänge**
Keine vorhanden.

### **Ereignis-Ausgänge**
Keine vorhanden.

### **Daten-Eingänge**
Keine vorhanden.

### **Daten-Ausgänge**
Keine vorhanden.

### **Adapter**
Keine vorhanden.

## Funktionsweise

Der Baustein definiert eine global konstante Variable `BACKGROUND_COLOUR` vom Typ `USINT` mit dem Initialwert `1`. Diese Konstante repräsentiert den Farbindex für den Hintergrund einer Softkey-Maske. Durch die Bereitstellung als globale Konstante kann dieser Wert an verschiedenen Stellen im Projekt referenziert werden, ohne dass er mehrfach deklariert werden muss. Dadurch wird die Wartbarkeit und Konsistenz in der Softwareentwicklung erhöht.

## Technische Besonderheiten

- **Typ**: `GlobalConstants` (kein Funktionsblock im klassischen Sinne).
- **Initialwert**: Die Konstante ist mit dem Wert `USINT#1` vordefiniert.
- **Kommentar**: Der Kommentar `1: AID_SKM_BACKGROUND_COLOUR - Background colour index.` erläutert die Bedeutung des Wertes.
- **Herkunft**: Der Baustein gehört zum Paket `isobus::UT::Q::const::AID` und ist Teil einer ISOBUS-spezifischen Bibliothek.

## Zustandsübersicht

Nicht anwendbar – es existieren keine Zustände, da es sich um reine Konstanten handelt.

## Anwendungsszenarien

- **ISOBUS-Steuerterminals**: Verwendung in HMI-Anwendungen, um einheitliche Farbdefinitionen für Softkey-Masken zu referenzieren.
- **Projektentwicklung**: Als zentrale Konstante, um den Farbindex für Hintergründe an zentraler Stelle zu definieren und spätere Änderungen zentral vornehmen zu können.
- **Code-Generierung**: Der Baustein wird typischerweise in Verbindung mit von der 4diac-IDE generiertem Code verwendet, um Konstanten in die Zielsprache (z. B. C++) zu übernehmen.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu herkömmlichen Funktionsbausteinen, die Daten verarbeiten und Ereignisse auslösen, bietet `AID_SKM` ausschließlich Konstanten an. Vergleichbare Bausteine könnten andere globale Konstanten-Container sein (z. B. `AID_OBJ`, `AID_PROP`), die jeweils unterschiedliche Attribut-IDs für verschiedene Objekttypen bereitstellen. Der Vorteil eines solchen Containers liegt in der zentralen Verwaltung aller relevanten IDs, wodurch Redundanz vermieden wird.

## Fazit

Der `AID_SKM`-Baustein ist ein schlankes Konstantenmodul, das eine spezifische Attribut-ID für Softkey-Masken bereitstellt. Er ist ein wesentlicher Bestandteil einer ISOBUS-Anwendung, um Farbwerte einheitlich zu definieren und die Softwarearchitektur übersichtlich zu halten. Durch seine einfache Struktur eignet er sich gut für die direkte Verwendung in 4diac-Projekten, in denen globale Konstanten benötigt werden.
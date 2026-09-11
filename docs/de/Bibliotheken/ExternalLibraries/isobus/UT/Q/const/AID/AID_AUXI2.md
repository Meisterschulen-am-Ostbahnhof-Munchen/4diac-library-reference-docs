# AID_AUXI2

![AID_AUXI2](./AID_AUXI2.svg)

* * * * * * * * * *
## Einleitung

AID_AUXI2 ist ein GlobalConstants-Datentyp im Rahmen der 4diac-IDE, der konstante Attribut-IDs für das Auxiliary Input Type 2 (AUXI2) Objekt im ISOBUS-Protokoll (ISO 11783) definiert. Die Konstanten werden zentral verwaltet und ermöglichen eine standardisierte Referenzierung der Objektattribute in anderen Bausteinen.

Das Modul wurde von HR Agrartechnik GmbH entwickelt, Version 1.0, erstellt von Franz Höpfinger am 20.06.2026. Es steht unter der Eclipse Public License 2.0 (EPL-2.0).

## Schnittstellenstruktur

Da es sich um eine GlobalConstants-Definition handelt, existieren keine typischen Funktionsblock-Schnittstellen wie Ereignis- oder Datenein-/ausgänge. Stattdessen werden zwei globale Konstanten bereitgestellt, die in beliebigen Bausteinen referenziert werden können.

### **Ereignis-Eingänge**
Nicht vorhanden.

### **Ereignis-Ausgänge**
Nicht vorhanden.

### **Daten-Eingänge**
Nicht vorhanden.

### **Daten-Ausgänge**
Nicht vorhanden (die Konstanten sind global und können überall gelesen werden).

### **Adapter**
Nicht vorhanden.

**Global Konstanten:**

| Name | Typ | Initialwert | Beschreibung |
|------|-----|-------------|--------------|
| `BACKGROUND_COLOUR` | USINT | `USINT#1` | 1: AID_AUXI2_BACKGROUND_COLOUR – Hintergrundfarb-Index. |
| `FUNC` | USINT | `USINT#2` | 2: AID_AUXI2_FUNC – Bitmaske: Bits 0–4 = Auxiliary-Funktionstyp (0–14), Bit 5 = Critical Control, Bit 6 = reserviert (0), Bit 7 = Single-Assignment. |

## Funktionsweise

Die Konstanten definieren die Attribut-IDs, die für das Auxiliary Input Type 2 Objekt im ISOBUS verwendet werden. `BACKGROUND_COLOUR` codiert die ID für den Hintergrundfarb-Index (Wert 1), während `FUNC` die ID für die Funktions-Bitmaske (Wert 2) festlegt. Diese IDs sind erforderlich, um auf die entsprechenden Attributwerte im Objekt zuzugreifen oder diese zu setzen. Durch die zentrale Definition wird eine einheitliche und fehlerresistente Nutzung im gesamten Projekt ermöglicht.

## Technische Besonderheiten

- **Paketstruktur:** Die Konstanten sind im Paket `isobus::UT::Q::const::AID` organisiert, um eine klare Zuordnung zum ISOBUS-Kontext zu gewährleisten.
- **Datentyp:** Beide Konstanten sind vom Typ `USINT` (Unsigned Short Integer), was eine effiziente Speicherung und Verarbeitung erlaubt.
- **Bitdefinition:** Die `FUNC`-Konstante enthält eine definierte Bitmaske mit besonderen Bedeutungen (Funktionstyp, Critical Control, reserviertes Bit, Single-Assignment). Diese Bitstruktur ist im ISOBUS-Standard festgelegt und muss exakt eingehalten werden.
- **Initialwerte:** Die Werte sind bereits als Konstanten vordefiniert und können nicht zur Laufzeit geändert werden.

## Zustandsübersicht

Für GlobalConstants existieren keine dynamischen Zustände. Die Werte sind statisch und unveränderlich, daher ist eine Zustandsüberwachung nicht erforderlich.

## Anwendungsszenarien

- **ISOBUS-basierte Steuerungen:** Einsatz in landwirtschaftlichen Maschinen, die das ISOBUS-Protokoll nutzen, insbesondere zur Anbindung von Auxiliary-Eingabegeräten (Typ 2).
- **Attribut-Referenzierung:** Andere Bausteine, die auf das Objekt `Auxiliary Input Type 2` zugreifen, können auf diese Konstanten zurückgreifen, um die Attribut-IDs eindeutig zu referenzieren.
- **Code-Wartung:** Durch die zentrale Definition wird vermieden, dass magische Zahlen im Code verstreut sind; Änderungen an den IDs müssen nur an einer Stelle vorgenommen werden.

## Vergleich mit ähnlichen Bausteinen

Ähnliche GlobalConstants-Datentypen existieren für andere ISOBUS-Objekte, z. B. `AID_AUXI1` oder `AID_VC` (Virtual Terminal). Diese weisen analoge Strukturen auf, unterscheiden sich jedoch in den spezifischen Attribut-IDs und deren semantischer Bedeutung. Während `AID_AUXI1` möglicherweise andere Funktionsbits definiert, umfasst `AID_AUXI2` zusätzlich Konzepte wie Critical Control und Single-Assignment. Die einheitliche Konvention erleichtert die Wiederverwendung und das Verständnis über verschiedene Module hinweg.

## Fazit

AID_AUXI2 stellt eine kompakte und klar definierte Sammlung von Attribut-IDs für das Auxiliary Input Type 2 Objekt bereit. Durch die Verwendung globaler Konstanten wird die Lesbarkeit und Wartbarkeit von ISOBUS-Anwendungen im 4diac-Umfeld verbessert. Die Dokumentation der Bitstruktur in der `FUNC`-Konstante erhöht die Transparenz und reduziert potenzielle Fehlkonfigurationen. Insgesamt ist der Baustein eine sinnvolle Grundlage für die Entwicklung robuster ISOBUS-Kommunikationslösungen in der Agrartechnik.
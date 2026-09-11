# IG5_NAME_Functions

![IG5_NAME_Functions](./IG5_NAME_Functions.svg)

* * * * * * * * * *

## Einleitung

`IG5_NAME_Functions` ist eine Sammlung globaler Konstanten für die ISO 11783 (ISOBUS) Kommunikation, speziell für die **Industry Group 5** (Industrieprozesssteuerung, insbesondere stationäre Generatoren (Gen-Sets) und Bohrlochstimulationspumpen). Die Konstanten definieren Funktionscodes, die in bestimmten PGNs (Parameter Group Numbers) verwendet werden, um den Zweck eines angeschlossenen Geräts innerhalb des ISOBUS-Netzwerks zu identifizieren. Die Definitionen folgen dem Standard IEC 61499-1, jedoch handelt es sich nicht um einen Funktionsblock mit ausführbarer Logik, sondern um eine verteilbare Konstantenressource für die 4diac-IDE.

## Schnittstellenstruktur

Da es sich um eine globale Konstantensammlung handelt, besitzt dieser Baustein keine klassischen Ein-/Ausgangs‑Schnittstellen.

### **Ereignis-Eingänge**

Nicht vorhanden.

### **Ereignis-Ausgänge**

Nicht vorhanden.

### **Daten-Eingänge**

Nicht vorhanden.

### **Daten-Ausgänge**

Die definierten Konstanten können als Datenwerte angesehen werden, die in anderen FB‑Bausteinen referenziert werden. Die folgende Tabelle listet alle Konstante auf:

| Konstante | Typ | Wert | Beschreibung |
|-----------|-----|------|--------------|
| `F_INDUSTRIAL_GEN_SET_SUPPLEMENTAL_ENGINE_CONTROL_SENSING` | `BYTE` | 128 | Zusätzliche Motorsteuerungssensorik (Gen-Sets) |
| `F_INDUSTRIAL_GEN_SET_GENERATOR_SET_CONTROLLER` | `BYTE` | 129 | Generator‑Set‑Controller (Generatoreinheit) |
| `F_INDUSTRIAL_GEN_SET_GENERATOR_VOLTAGE_REGULATOR` | `BYTE` | 130 | Generator‑Spannungsregler |
| `F_INDUSTRIAL_GEN_SET_CHOKE_ACTUATOR` | `BYTE` | 131 | Drosselklappen‑Stellantrieb für Gas‑Motoren |
| `F_WELL_STIMULATION_PUMP` | `BYTE` | 132 | Pumpe zur Bohrlochstimulation (Öl‑/Gasförderung) |
| `F_INDUSTRIAL_GEN_SET_NOT_AVAILABLE` | `BYTE` | 255 | Nicht verfügbar (vorläufige Zuordnung) |
| `F_NOT_AVAILABLE` | `BYTE` | 255 | Allgemein nicht verfügbar |

### **Adapter**

Nicht vorhanden.

## Funktionsweise

Die globalen Konstanten werden in einem ISOBUS‑Netzwerk verwendet, um die Funktion eines angeschlossenen Geräts (z. B. eines Generators oder einer Pumpe) zu kennzeichnen. Die Werte entsprechen den festgelegten **Function Codes** der ISOBUS‑Telegramme. Jede Konstante liefert einen unveränderlichen Byte‑Wert, der in anderen Funktionsblöcken direkt als Vergleichs‑ oder Zuordnungswert eingesetzt werden kann. Die Liste ist bewusst statisch gehalten, um eine klare und fehlerfreie Zuordnung im gesamten Steuerungsprojekt zu gewährleisten.

## Technische Besonderheiten

- Die Konstanten sind als `BYTE` (8‑Bit) definiert und decken den Bereich 128–132 sowie 255 ab.
- Es existieren zwei unterschiedliche Konstanten mit demselben Wert `255` – `F_INDUSTRIAL_GEN_SET_NOT_AVAILABLE` (für Gen‑Sets) und `F_NOT_AVAILABLE` (allgemein). Dies erlaubt eine differenzierte Verwendung in verschiedenen Kontexten.
- Die Konstanten sind als **globale Konstanten** in der 4diac‑IDE verfügbar und können projektweit referenziert werden, ohne sie erneut deklarieren zu müssen.
- Die Definition ist als `GlobalConstants`‑Ressource angelegt und besitzt eine Versionierung (`Version 1.0`) sowie eine Zuordnung zum ISOBUS‑PGN‑Paket `isobus::pgn::const`.

## Zustandsübersicht

Da keine ausführbare Logik oder Zustandsautomaten vorhanden sind, entfällt eine Zustandsübersicht.

## Anwendungsszenarien

- **Steuerung von Generatorsätzen:** Verwendung der Konstanten, um die Funktion eines angeschlossenen Geräts (z. B. Motorsteuerung, Spannungsregler) im ISOBUS‑Netzwerk zu identifizieren.
- **Industrieprozess‑Steuerung:** Einsatz in stationären Anlagen, um verschiedene Komponenten (z. B. Drosselklappen‑Aktuatoren) eindeutig zu klassifizieren.
- **Öl‑ und Gasindustrie:** Die Konstante `F_WELL_STIMULATION_PUMP` ermöglicht die Zuordnung von Steuerungsdaten für Pumpen, die in der Bohrlochstimulation eingesetzt werden.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu typischen Funktionsblöcken besitzt `IG5_NAME_Functions` keine Schnittstellen oder Verarbeitungslogik. Es handelt sich um eine **reine Datenressource**, vergleichbar mit einem Header‑File in klassischen Programmierumgebungen. Andere GS‑Konstanten (z. B. für andere Industry Groups) folgen demselben Muster, unterscheiden sich jedoch in den Wertebereichen oder Namenskonventionen.

## Fazit

`IG5_NAME_Functions` stellt eine standardisierte und versionierte Sammlung von Funktionscodes für die ISOBUS‑Industriegruppe 5 bereit. Durch die Verwendung globaler Konstanten wird eine hohe Wiederverwendbarkeit und Klarheit in 4diac‑Projekten erreicht, insbesondere für Anwendungen im Bereich stationärer Generatoren und Bohrlochstimulation. Die klare Struktur erleichtert die Wartung und Erweiterung der Kommunikationsprotokolle.

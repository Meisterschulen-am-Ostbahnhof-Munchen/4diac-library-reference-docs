# AID_TYPE

![AID_TYPE](./AID_TYPE.svg)

* * * * * * * * * *

## Einleitung

Der Baustein **AID_TYPE** ist ein **GlobalConstants**-Typ im Rahmen der 4diac-IDE. Er definiert eine globale Konstante, die als Attribut-Identifikator für Objekttypen im ISOBUS-Kontext (Universal Terminal, UT) dient. Die Konstante gehört zur Paketstruktur `isobus::UT::Q::const::AID` und stellt eine numerische Kennung für die Abfrage des Objekttyp-Attributs bereit.

## Schnittstellenstruktur

Da es sich um einen globalen Konstanten-Baustein handelt, besitzt er keine klassischen Ein-/Ausgangsschnittstellen wie Funktionsblöcke (FB) oder Adapter. Die Konstante wird über den globalen Symbolnamen `AID_TYPE.OBJ_TYPE` referenziert und ist in jedem Baustein des Projekts sichtbar, sobald der Konstanten-Baustein eingebunden ist.

### **Ereignis-Eingänge**

Keine

### **Ereignis-Ausgänge**

Keine

### **Daten-Eingänge**

Keine

### **Daten-Ausgänge**

Keine (die Konstante wird direkt über den Namen angesprochen, nicht als Ausgang)

### **Adapter**

Keine

## Funktionsweise

Der Baustein stellt eine einzige, unveränderliche Konstante bereit:

- `OBJ_TYPE` vom Typ `USINT` (8‑Bit unsigned integer) mit dem initialen Wert `0`.

Diese Konstante repräsentiert die **AID_TYPE_OBJ_TYPE** – eine Kennung, die angibt, dass es sich um das Attribut „Objekttyp“ handelt. Im ISOBUS-Protokoll wird diese Kennung verwendet, um Abfragen oder Benachrichtigungen bezüglich des Objekttyps eines Elements (z. B. eines Arbeitsbildschirms) zu identifizieren.

Der Wert wird zur Compile-Zeit festgelegt und ist damit für alle weiteren Bausteine unveränderlich. Dadurch wird eine konsistente und fehlerfreie Referenzierung innerhalb des ISOBUS-Programms gewährleistet.

## Technische Besonderheiten

- **Typ**: `USINT` (Unsigned Short Integer) – Bereich 0–255.
- **Initialwert**: `0` – entspricht der ISO 11783-Definition für „Object Type identifier“.
- **Paketstruktur**: `isobus::UT::Q::const::AID` – deutet auf die Zugehörigkeit zu einer ISOBUS-Umsetzung hin.
- **Originalcode**: Der Quellcode ist in ST (Structured Text) verfasst und als globaler Konstantenblock definiert.
- **Eigenschaften**: Der Baustein ist als `VAR_GLOBAL CONSTANT` deklariert, d. h., er ist projektweit gültig und nicht änderbar.

## Zustandsübersicht

Nicht zutreffend – ein GlobalConstants-Baustein besitzt keine internen Zustände oder Zustandsautomaten. Er dient ausschließlich der symbolischen Definition eines konstanten Wertes.

## Anwendungsszenarien

- **ISOBUS Universal Terminal (UT)**: Erkennung und Verarbeitung von Objekttyp-Attributen in Steuerungssystemen.
- **Normgerechte Kommunikation**: Nutzung der vordefinierten AID-Kennung für das ISOBUS-Protokoll.
- **Referenzierung**: In anderen Funktionsblöcken oder Applikationen kann `AID_TYPE.OBJ_TYPE` direkt als Index oder Parameter verwendet werden.
- **Entwicklung landwirtschaftlicher Steuerungen**: Einsatz in Traktoren, Anbaugeräten oder Bedienterminals mit ISOBUS-Anbindung.

## Vergleich mit ähnlichen Bausteinen

Es gibt zahlreiche GlobalConstants-Bausteine in ISOBUS-Umsetzungen, die jeweils spezifische Attribut-IDs (AIDs) für unterschiedliche Abfragen definieren. Im Gegensatz zu reinen Funktionsblöcken bieten Konstantenblöcke keine Logik, sondern nur Daten. Im Vergleich zu anderen Konstantenbausteinen, die z. B. Farbwerte oder Puffergrößen festlegen, ist dieser auf die Objekttyp-Identifikation spezialisiert und folgt strikt den ISOBUS-Vorgaben.

## Fazit

Der Baustein `AID_TYPE` ist eine einfache, aber essenzielle Komponente für ISOBUS-basierte Anwendungen. Er stellt eine klar definierte Kennung für den Objekttyp bereit und trägt so zur Interoperabilität und Standardkonformität der Steuerungssoftware bei. Seine Verwendung als globale Konstante erleichtert die Wartung und Vermeidung von Magic Numbers im Code.

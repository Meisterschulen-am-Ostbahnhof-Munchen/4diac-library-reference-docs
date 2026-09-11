# IG0_Device_Classes

![IG0_Device_Classes](./IG0_Device_Classes.svg)

* * * * * * * * * *
## Einleitung

Der Baustein **IG0_Device_Classes** ist eine globale Konstantendefinition im 4diac-IDE, die spezifische Geräteklassen (Device Classes) für die Fahrzeugsysteme der ISOBUS-Industriegruppe 0 (Industry Group 0) bereitstellt. Diese Konstanten werden verwendet, um Fahrzeugsysteme eindeutig zu identifizieren und zu klassifizieren, insbesondere in Anwendungen, die auf dem ISOBUS-Protokoll basieren. Die Definition umfasst zwei Konstanten, die den Zustand „Nicht-spezifisches System“ und „Nicht verfügbar“ repräsentieren.

## Schnittstellenstruktur

Da es sich um eine globale Konstantendeklaration handelt, besitzt der Baustein keine Ereignis- oder Datenschnittstellen im Sinne eines Funktionsblocks. Die Struktur ist rein deklarativ und stellt Werte für die Verwendung in anderen Bausteinen bereit.

### **Ereignis-Eingänge**
- Keine

### **Ereignis-Ausgänge**
- Keine

### **Daten-Eingänge**
- Keine (globale Konstanten, nicht als Eingänge gebunden)

### **Daten-Ausgänge**
- Keine (die Werte werden direkt über den Namen referenziert)

### **Adapter**
- Keine

## Funktionsweise

Die globalen Konstanten `DC_NON_SPECIFIC_SYSTEM` und `DC_NOT_AVAILABLE` sind vom Typ `BYTE` und tragen die Werte 0 bzw. 127. Sie werden im Anwendungscode über ihren qualifizierten Namen (z.B. `IG0_Device_Classes.DC_NON_SPECIFIC_SYSTEM`) aufgerufen und können für Vergleiche oder Zuweisungen verwendet werden. Die Konstanten sind als `VAR_GLOBAL CONSTANT` deklariert, d.h. ihre Werte sind unveränderlich und stehen im gesamten Projekt zur Verfügung. Sie dienen als Standardwerte für Geräteklassen, wenn kein spezifisches System angegeben ist oder die Information nicht verfügbar ist.

## Technische Besonderheiten

- **Typ:** `BYTE` – ein 8-Bit-Unsigned-Integer, geeignet für Werte von 0 bis 255.
- **Initialwerte:** 
  - `DC_NON_SPECIFIC_SYSTEM = 0` – kennzeichnet ein unspezifisches oder generisches Fahrzeugsystem.
  - `DC_NOT_AVAILABLE = 127` – kennzeichnet, dass die Geräteklasse nicht verfügbar ist.
- **Gültigkeitsbereich:** Global – die Konstanten sind im gesamten Projekt nutzbar.
- **Standardkonformität:** Die Deklaration folgt dem IEC 61499-1 Standard und spezifiziert den Paketnamen `isobus::pgn::const`.

## Zustandsübersicht

Ein Zustandsdiagramm ist nicht anwendbar, da der Baustein keine Laufzeitlogik besitzt. Die Konstanten repräsentieren statische Werte und haben keinen Zustandsübergang.

## Anwendungsszenarien

- **ISOBUS-Kommunikation:** Verwendung in Protokollnachrichten, um die Geräteklasse eines angeschlossenen Fahrzeugsystems zu setzen oder zu interpretieren.
- **Systemkonfiguration:** In Steuerungsanwendungen, um zwischen „nicht spezifiziertem“ und „nicht verfügbarem“ System zu unterscheiden.
- **Fallback-Logik:** Als Standardwerte, wenn keine konkreten Geräteklassen-Informationen vorliegen.

## Vergleich mit ähnlichen Bausteinen

Ähnliche globale Konstantendefinitionen existieren für andere Industriegruppen (z.B. `IG1_Device_Classes` für Industriegruppe 1). Im Gegensatz zu einem Funktionsblock bietet `IG0_Device_Classes` keinerlei Verarbeitungslogik, sondern stellt lediglich Werte bereit. Es kann als Gegenstück zu einer Enumerations- oder Konstantenliste in klassischen Programmiersprachen betrachtet werden, die in 4diac als globaler Konstantenblock modelliert ist.

## Fazit

Der Baustein `IG0_Device_Classes` ist eine einfache, aber wichtige Deklaration für die ISOBUS-Integration in Fahrzeugsystemen. Er stellt zwei universelle Konstanten bereit, die eine einheitliche Behandlung von unspezifischen und nicht verfügbaren Geräteklassen ermöglichen. Trotz fehlender interner Logik trägt er zur Standardisierung und Interoperabilität in der Automatisierungstechnik bei und ist ein grundlegendes Element für die Entwicklung ISOBUS-konformer Applikationen.
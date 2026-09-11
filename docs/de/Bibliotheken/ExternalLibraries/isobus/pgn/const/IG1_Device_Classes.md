# IG1_Device_Classes

![IG1_Device_Classes](./IG1_Device_Classes.svg)

* * * * * * * * * *

## Einleitung

Der Baustein **IG1_Device_Classes** definiert globale Konstanten für die Fahrzeugklassen (Device Classes) der Industry Group 1 (IG1) gemäß dem ISOBUS-Standard (ISO 11783). Er dient als zentrale, wiederverwendbare Konstantendefinition, um einheitliche Werte für verschiedene Fahrzeugsysteme bereitzustellen.

## Schnittstellenstruktur

Der Baustein besitzt keine dynamischen Schnittstellen (Ereignis- oder Dateneingänge/-ausgänge). Er stellt ausschließlich globale Konstanten bereit, die in anderen Bausteinen und Anwendungen referenziert werden können.

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

**IG1_Device_Classes** enthält eine Liste von Konstanten vom Typ `BYTE`, die die standardisierten Fahrzeugklassenkennungen für die Industriegruppe 1 (Fahrzeugsysteme) abbilden. Die Konstanten werden zur Laufzeit in ISOBUS-Nachrichten (z.B. in Parameter Group Numbers) verwendet, um die Art des Fahrzeugs zu identifizieren. Jede Konstante besitzt einen symbolischen Namen und einen eindeutigen Wert.

Die definierten Konstanten sind:

- `DC_NON_SPECIFIC_SYSTEM` (Wert 0): Allgemeines, nicht spezifiziertes System.
- `DC_TRACTOR` (Wert 1): Traktor.
- `DC_TRAILER` (Wert 2): Anhänger.
- `DC_NOT_AVAILABLE` (Wert 127): Wert nicht verfügbar (für Fehlerbehandlung oder unbekannte Systeme).

## Technische Besonderheiten

- **Paketzuordnung:** Die Konstanten sind dem Paket `isobus::pgn::const` zugeordnet, was die eindeutige Identifizierung innerhalb des ISOBUS-Kontexts erleichtert.  
- **Datentyp:** Alle Werte sind als `BYTE` deklariert (8-Bit).  
- **Organisatorische Herkunft:** Die Konfiguration stammt ursprünglich vom Hersteller „Demmler Andreas Fahrzeugbau“ und wurde von Franz Höpfinger erstellt.  
- **Standard-Konformität:** Die Werte entsprechen den ISO 11783- (ISOBUS-) Vorgaben für Fahrzeugklassen der Industriegruppe 1.

## Zustandsübersicht

Nicht anwendbar, da der Baustein keine Zustandsmaschine oder dynamische Prozesse besitzt.

## Anwendungsszenarien

- **ISOBUS-Anwendungen:** Nutzung in Steuergeräten oder Anzeigeinheiten, um Fahrzeugtypen zu unterscheiden (z.B. zur Anzeige von Traktor- oder Anhängerdaten).  
- **Datenvalidierung:** Überprüfung von eingehenden ISOBUS-Nachrichten auf gültige Fahrzeugklassenwerte.  
- **Konfigurationsmodule:** Bereitstellung von Auswahlmöglichkeiten für Fahrzeugklassen in Einstellungs- oder Diagnoseoberflächen.

## Vergleich mit ähnlichen Bausteinen

Da es sich um eine reine Konstantendefinition handelt, gibt es keine direkten bausteinartigen Alternativen. Andere Bausteine könnten jedoch ähnliche Konstantengruppen für andere Industriegruppen (z.B. IG0, IG2) bereitstellen. Diese werden oft ebenfalls als Global Constants implementiert, um eine konsistente Wertbasis über verschiedene Module hinweg zu gewährleisten.

## Fazit

**IG1_Device_Classes** ist eine unverzichtbare Grundlage für ISOBUS-Anwendungen, die Fahrzeugklassen gemäß der Industriegruppe 1 unterstützen. Durch die zentrale Definition als globale Konstanten wird eine klare, wartbare und fehlerresistente Codierung ermöglicht. Die einfache Struktur ohne Schnittstellen macht den Baustein leicht verständlich und ideal für die Wiederverwendung in vielfältigen Kontexten.

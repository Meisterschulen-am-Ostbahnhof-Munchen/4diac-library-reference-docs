# IG1_Source_Addresses

![IG1_Source_Addresses](./IG1_Source_Addresses.svg)

* * * * * * * * * *

## Einleitung

Der Baustein **IG1_Source_Addresses** ist kein klassischer Funktionbaustein (FB) im Sinne eines prozessverarbeitenden Elements, sondern eine Sammlung **globaler Konstanten** (GlobalConstants) für die ISOBUS-Kommunikation (ISO 11783). Er definiert feste Quelladressen (Source Addresses, SA) für Geräte der **Industry Group 1** (landwirtschaftliche und mobile Maschinen). Diese Konstanten werden typischerweise in FBs verwendet, die auf dem ISOBUS kommunizieren, um eindeutige Adressen für Sender (z. B. Steuergeräte, Sensoren) innerhalb des Netzwerks bereitzustellen.

Die Konstanten repräsentieren die **offiziellen SA-Werte**, die von SAE (Society of Automotive Engineers) für unterschiedliche Fahrzeug- und Anhänger-Subsysteme vergeben wurden. Der Baustein selbst besitzt keine Laufzeitlogik, sondern stellt lediglich eine zentrale, wiederverwendbare Datenbasis bereit.

## Schnittstellenstruktur

Da es sich um eine Gruppe von globalen Konstanten handelt, besitzt der Baustein **keine** ereignis- oder datenorientierten Schnittstellen im herkömmlichen Sinn. Stattdessen werden die Werte direkt über die **Konstantennamen** referenziert, z. B. `SA_AUTOMATED_DRIVING_CONTROLLER_1` in anderen Bausteinen.

### **Ereignis-Eingänge**

Keine vorhanden.

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

Keine Eingänge vorhanden.

### **Daten-Ausgänge**

Die globalen Konstanten dienen als **quasi-Ausgabewerte**. Sie können in einem FB über den jeweiligen Konstantennamen gelesen werden und liefern den entsprechenden SA-Wert (Datentyp `BYTE`). Eine Auflistung aller Konstanten findet sich im Abschnitt [Funktionsweise](#funktionsweise) (Tabelle der wichtigsten Werte).

### **Adapter**

Keine Adapter vorhanden.

## Funktionsweise

Der Baustein definiert insgesamt **87 Konstanten** (im Wertebereich 128 bis 247), die die offiziellen Quelladressen für verschiedene ISOBUS-Geräte der Industry Group 1 festlegen. Die Konstanten sind ausschließlich lesbar und werden zur Kompilierzeit in andere Bausteine eingebunden.

Hier eine Auswahl der wichtigsten Adressen (vollständige Liste siehe `OriginalSource` der XML):

| Konstante | Wert (dez) | Beschreibung |
|-----------|-----------|--------------|
| `SA_AUTOMATED_DRIVING_CONTROLLER_1` | 158 | Automatischer Fahrcontroller 1 |
| `SA_AUTOMATED_DRIVING_CONTROLLER_2` | 156 | Automatischer Fahrcontroller 2 |
| `SA_ELECTRIC_PROPULSION_CONTROL_UNIT_1` | 239 | Elektrische Antriebssteuerung 1 |
| `SA_ADVANCED_EMERGENCY_BRAKING_SYSTEM` | 160 | Notbremssystem |
| `SA_TRAILER_1_BRIDGE` | 200 | Anhänger 1 – Brücke |
| `SA_TRAILER_2_BRAKES_ABS_EBS` | 194 | Anhänger 2 – Bremsen |
| `SA_BATTERY_PACK_MONITOR_1` | 243 | Batteriepack-Überwachung 1 |
| `SA_AUXILIARY_POWER_UNIT_APU_1` | 247 | Hilfsaggregat (APU) 1 |
| `SA_TACHOGRAPH` | 238 | Fahrtenschreiber |
| ... | ... | ... |

Die Werte sind als `BYTE`-Konstanten definiert und können direkt in arithmetischen oder logischen Ausdrücken sowie in Kommunikationsbausteinen (z. B. CAN-Sendern) verwendet werden.

## Technische Besonderheiten

- **Konformität:** Die Konstanten entsprechen den SAE-Adressvorgaben für ISOBUS gemäß ISO 11783-7.
- **Datentyp:** Alle Konstanten sind vom Typ `BYTE` (8-Bit-Ganzzahl).
- **Wertebereich:** Die Adressen liegen im erlaubten Bereich von 128 bis 247 (für Industry Group 1).
- **Verwendung:** Da es sich um `CONSTANT`-Deklarationen handelt, können die Werte nicht zur Laufzeit verändert werden; sie sind fest eingebettet.
- **Reservierte Bereiche:** Einige Adressen (z. B. 208–227) sind für zukünftige SAE-Zuordnungen reserviert und ebenfalls als Konstanten definiert.
- **Portabilität:** Die Konstanten sind in einem separaten Paket (`isobus::pgn::const`) organisiert und können in alle FBs des Projekts importiert werden.

## Zustandsübersicht

Der Baustein besitzt **keine Zustände** – es handelt sich um eine rein deklarative Definition. Es gibt keine Zustandsautomaten, Start-/Stopp-Aktionen oder zeitliche Abläufe. Die Funktion ist deterministisch und zur Laufzeit unveränderlich.

## Anwendungsszenarien

- **ISOBUS-Kommunikation:** Einsatz in FBs, die Nachrichten über den CAN-Bus senden und dabei eine eigene, eindeutige Quelladresse benötigen. Die Konstanten verhindern hardcodierte Werte und erhöhen die Wartbarkeit.
- **Fahrzeugsteuerung:** Zuweisung der SA für Steuergeräte wie Motorsteuerung, Bremsen, Anhänger-Interfaces oder Fahrerassistenzsysteme.
- **Diagnose:** Nutzung des WWH-OBD-Testers (SA 241) in Diagnosefunktionen.
- **Anhänger- und Trailermanagement:** Adressierung mehrerer Anhänger (bis zu 5) mit eigenen Brücken, Licht-, Brems- und Cargo-Adressen.

## Vergleich mit ähnlichen Bausteinen

Es gibt weitere Konstantenpakete in ISOBUS, z. B. für andere Industry Groups (IG0, IG2, IG3) oder für spezifische Parameter-Gruppen (PGN). Der Unterschied besteht in den zugewiesenen Adressbereichen und den verwendeten Gerätetypen. Diese Konstantengruppe ist speziell für die Landtechnik (IG1) optimiert und enthält typische landwirtschaftliche Geräte.

Gegenüber einem FB, der z. B. eine dynamische Adressvergabe implementiert, bietet dieser Baustein **statische, festgelegte Werte** – ideal für Systeme mit festen Rollen. Für dynamische Konfigurationen müssten zusätzliche Logikbausteine verwendet werden.

## Fazit

Der Baustein **IG1_Source_Addresses** ist eine unverzichtbare, weil zentrale Datenquelle für die ISOBUS-Entwicklung in der 4diac-IDE. Er bündelt die offiziellen Quelladressen der Industry Group 1 in einer sauberen, wiederverwendbaren Form. Auch wenn er selbst keine aktive Steuerlogik enthält, vereinfacht er die korrekte und konsistente Adressierung erheblich und beugt Fehlern durch hardcodierte Werte vor. Durch seine klare Struktur und die umfassenden Kommentare ist er sowohl für Neulinge als auch für erfahrene Entwickler leicht nachvollziehbar und erweiterbar.

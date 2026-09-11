# IG2_NAME_Functions

![IG2_NAME_Functions](./IG2_NAME_Functions.svg)

* * * * * * * * * *

## Einleitung

Die globale Konstantendefinition `IG2_NAME_Functions` stellt eine Sammlung von Funktionscodes für die ISO 11783-Identifikation (ISOBUS NAME) bereit. Sie definiert die numerischen Werte (als `BYTE`) für die unterschiedlichen Steuerungs- und Messfunktionen, die in landwirtschaftlichen Maschinen und Geräten verwendet werden. Diese Konstanten werden typischerweise in Softwareprojekten eingesetzt, die ISOBUS-kompatible Steuergeräte implementieren oder mit deren Nachrichten umgehen.

## Schnittstellenstruktur

Da es sich um einen `GlobalConstants`-Baustein handelt, besitzt er keine Ereignis- oder Datenports. Die folgenden Abschnitte sind daher nicht anwendbar.

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

Die Konstanten dienen als symbolische Namen für Werte, die im ISO 11783-Standard (ISOBUS) für die Klassifizierung von Funktionen verwendet werden. Sie sind im Namespace `isobus::pgn::const` definiert und können direkt in IEC 61499- oder IEC 61131-Programmen referenziert werden, ohne dass magische Zahlen verwendet werden müssen. Jede Konstante ist eindeutig benannt und beinhaltet in ihrem Namen den Bezug auf das jeweilige System (z. B. Traktor, Pflanzenschutz, Erntemaschine) und die Funktion (z. B. Maschinensteuerung, Durchflussmessung).

Die Werte sind als `BYTE` (0-255) definiert. Der Wert 255 wird generell für „Nicht verfügbar“ verwendet, während andere Werte spezifischen Funktionen zugeordnet sind. Die Zuordnung folgt den Tabellen des ISO 11783-5 Standards für NAME-Datenelemente.

## Technische Besonderheiten

- Die Konstanten sind als globale Konstanten deklariert und stehen damit in allen Bausteinen und Programmen zur Verfügung, die das Paket einbinden.
- Die Namen sind hierarchisch aufgebaut: `F_<SYSTEM>_<FUNKTION>`, wobei `SYSTEM` das jeweilige Maschinensystem (z. B. `TRACTOR`, `SPRAYERS`) und `FUNKTION` die spezifische Aufgabe (z. B. `MACHINE_CONTROL`, `PRODUCT_FLOW`) kennzeichnet.
- Viele Systeme haben gemeinsame Funktionscodes (z. B. Wert 132 für „Maschinensteuerung“), aber durch die Systemkomponente im Namen wird Eindeutigkeit gewährleistet.
- Die Werte entsprechen den offiziellen Definitionen der ISO 11783, Abschnitt 5 (Datenidentifikation).

## Zustandsübersicht

Nicht zutreffend – ein `GlobalConstants`-Baustein besitzt keine Zustände oder Zustandsautomaten.

## Anwendungsszenarien

- **ISOBUS-Steuergeräteentwicklung**: Verwendung der Konstanten zum Setzen des Funktionscodes im NAME-Eintrag eines Steuergeräts.
- **Nachrichtenanalyse**: Interpretation von NAME-Nachrichten, die von anderen Geräten empfangen werden, mithilfe der Konstanten.
- **Programmierung von ISOBUS-Anwendungen**: Einbinden der Konstantendatei in ein Projekt, um Funktionen wie „Saatmengenregelung“ oder „Produktflussmessung“ eindeutig zu identifizieren.
- **Diagnose-Tools**: Anzeige der Funktion eines entfernten Geräts basierend auf den empfangenen NAME-Informationen.

## Vergleich mit ähnlichen Bausteinen

Im ISOBUS-Umfeld gibt es weitere Global-Constants-Definitionen, z. B. für Geräteklassen (Device Class), Industry Groups oder andere Datenelemente. `IG2_NAME_Functions` ist speziell auf die Funktionscodes der Industry Group 2 (landwirtschaftliche Maschinen) ausgerichtet. Andere Konstantensammlungen könnten allgemeinere oder zusätzliche kodierte Werte enthalten, aber diese bieten eine vollständige, standardkonforme Abdeckung der wichtigsten Funktionen.

## Fazit

`IG2_NAME_Functions` ist eine umfassende und klar strukturierte Konstantensammlung für ISOBUS-Funktionscodes. Sie erleichtert die Implementierung und Wartung von ISOBUS-Software erheblich, indem sie sprechende Namen für feste Werte bereitstellt und so Fehler durch falsche Zahlenwerte minimiert. Die Einhaltung der ISO 11783-Standards wird durch die direkte Übernahme der definierten Werte gewährleistet.

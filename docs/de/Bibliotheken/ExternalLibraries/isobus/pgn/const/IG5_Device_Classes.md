# IG5_Device_Classes

![IG5_Device_Classes](./IG5_Device_Classes.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `IG5_Device_Classes` ist eine Sammlung globaler Konstanten, die spezifische Geräteklassen (Device Classes) für die Industriegruppe 5 (Industry Group 5) im ISOBUS-Standard (ISO 11783) definiert. Diese Gruppe umfasst Fahrzeugsysteme. Die Konstanten dienen als Referenzwerte für die Klassifizierung von Geräten in landwirtschaftlichen und mobilen Maschinen, insbesondere im Bereich von Stromerzeugern (Gen-Sets) und für den Fall, dass keine Geräteklasse verfügbar ist.

## Schnittstellenstruktur

Da es sich um eine globale Konstantenliste handelt, besitzt der Baustein keine typischen Schnittstellen eines Funktionsblocks. Es werden keine Ereignisse oder Daten ausgetauscht, sondern die Konstanten werden direkt als symbolische Namen im Projekt verwendet.

### **Ereignis-Eingänge**

Nicht zutreffend – es sind keine Ereignis-Eingänge definiert.

### **Ereignis-Ausgänge**

Nicht zutreffend – es sind keine Ereignis-Ausgänge definiert.

### **Daten-Eingänge**

Nicht zutreffend – es sind keine Daten-Eingänge definiert.

### **Daten-Ausgänge**

Nicht zutreffend – es sind keine Daten-Ausgänge definiert. Stattdessen werden die Werte als globale Konstanten bereitgestellt, die an beliebigen Stellen im Projekt referenziert werden können.

### **Adapter**

Nicht zutreffend – es sind keine Adapter vorhanden.

## Funktionsweise

Der Baustein stellt zwei Konstanten vom Typ `BYTE` bereit:

- `DC_INDUSTRIAL_GEN_SET` mit dem Wert `0` – kennzeichnet ein stationäres Industrie-Prozesskontrollgerät (Gen-Set).
- `DC_NOT_AVAILABLE` mit dem Wert `127` – steht für "nicht verfügbar" und wird verwendet, wenn keine Geräteklasse angegeben werden kann.

Diese Konstanten werden typischerweise in ISOBUS-Anwendungen verwendet, um in Nachrichten oder Parametern die Geräteklasse zu referenzieren. Durch die Verwendung symbolischer Namen wird die Lesbarkeit des Codes verbessert und Fehler durch hartkodierte Zahlenwerte vermieden.

## Technische Besonderheiten

- Die Konstanten sind im Standard `61499-1` eingebettet und folgen der ISOBUS-Definition für Device Classes.
- Die Werte sind als `BYTE` deklariert und haben die Initialwerte `0` und `127`.
- Die Konstanten sind global gültig und können innerhalb des gesamten 4diac-Projekts verwendet werden.
- Die Definition basiert auf der ISOBUS-Spezifikation für Industry Group 5, die Fahrzeugsysteme betrifft.

## Zustandsübersicht

Da es sich um eine reine Konstantendefinition handelt, existiert keine Zustandsmaschine oder ein dynamisches Verhalten. Der Baustein ändert seinen Zustand nicht und benötigt keine Zustandsverwaltung.

## Anwendungsszenarien

- **Landwirtschaftliche Maschinen:** Verwendung zur Identifizierung eines angeschlossenen Geräts als stationäres Stromaggregat.
- **Fahrzeugkommunikation:** In ISOBUS-Nachrichten kann die Geräteklasse zur Laufzeit abgefragt und mit diesen Konstanten verglichen werden.
- **Fehlerbehandlung:** Wenn die Geräteklasse unbekannt ist, kann `DC_NOT_AVAILABLE` verwendet werden, um einen definierten Fallback zu bieten.
- **Konfiguration:** Die Konstanten können bei der Parametrierung von Steuergeräten eingesetzt werden, um feste Werte zu vermeiden.

## Vergleich mit ähnlichen Bausteinen

Im 4diac-Umfeld gibt es weitere GlobalConstants-Bausteine für andere Industriegruppen (z.B. IG0 bis IG4). Diese definieren jeweils spezifische Geräteklassen für ihre Gruppe. Der vorliegende Baustein ist auf Industriegruppe 5 fokussiert und bietet dort die relevanten Werte. Im Gegensatz zu Funktionsblöcken mit Ein-/Ausgängen handelt es sich hier um eine passive Datenquelle, die keine Logik ausführt.

## Fazit

`IG5_Device_Classes` ist ein einfacher, aber wichtiger Baustein für ISOBUS-Anwendungen, der die Verwendung standardisierter Geräteklassen erleichtert. Durch die klaren Konstantennamen wird die Wartbarkeit verbessert und die Einhaltung des ISOBUS-Standards unterstützt. Für Entwickler, die Fahrzeugsysteme der Industriegruppe 5 anbinden, stellt dieser Baustein eine nützliche Basis dar.

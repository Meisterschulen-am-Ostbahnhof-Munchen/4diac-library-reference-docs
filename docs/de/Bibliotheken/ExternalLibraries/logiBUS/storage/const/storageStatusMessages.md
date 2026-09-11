# storageStatusMessages

![storageStatusMessages](./storageStatusMessages.sv

* * * * * * * * * *

## Einleitung

Der Baustein `storageStatusMessages` ist ein globaler Konstantensatz der Bibliothek `logiBUS::storage::const`. Er definiert einheitliche Status- und Fehlermeldungen für Speicherzugriffe, insbesondere für die Initialisierung und die Nutzung von Non-Volatile Storage (NVS). Durch die zentrale Definition dieser Konstanten wird eine konsistente Kommunikation von Speicherzuständen innerhalb von 4diac-Anwendungen ermöglicht. Die Konstanten sind als `STRING`-Werte ausgeführt und können überall im Projekt referenziert werden.

## Schnittstellenstruktur

Da es sich um einen GlobalConstants-Baustein handelt, besitzt er keine Ereignis- oder Datenein-/ausgänge und auch keine Adapter. Die definierten Konstanten dienen als globale Datenquelle und sind für alle Funktionsblöcke der Anwendung lesbar.

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

Der Baustein stellt eine Sammlung von statischen Konstanten bereit, die zur Laufzeit nicht verändert werden können. Sie werden verwendet, um Rückgabewerte von Speicheroperationen zu standardisieren. Die folgende Tabelle listet alle Konstanten, ihre Werte und deren Bedeutung auf:

| Konstante             | Wert                                     | Bedeutung/Beschreibung                                      |
|-----------------------|------------------------------------------|-------------------------------------------------------------|
| `OK`                  | `'OK'`                                   | Erfolgreiche Speicherung oder allgemeiner Erfolg.           |
| `NO_CHANGE`           | `'Value was not stored. Old is new'`     | Der Wert wurde nicht geändert, der alte Wert bleibt aktiv.  |
| `INITIALISED`         | `'initialized'`                          | Speicher wurde erfolgreich initialisiert.                   |
| `NOT_INITIALISED`     | `'Not initialized'`                      | Speicher ist noch nicht initialisiert.                      |
| `ERR`                 | `'ERROR'`                                | Allgemeiner Fehler.                                         |
| `ERR_NVS_NOT_FOUND`   | `'ESP_ERR_NVS_NOT_FOUND'`                | Der angeforderten NVS-Schlüssel wurde nicht gefunden.       |
| `ERR_NVS_DEFAULT_SET` | `'Default Value was used'`               | Ein Standardwert wurde verwendet, da der eigentliche Wert fehlte. |
| `ERR_SECTION_EMPTY`   | `'SECTION empty'`                        | Die übergebene Sektion ist leer.                            |
| `ERR_KEY_EMPTY`       | `'KEY empty'`                            | Der übergebene Schlüssel ist leer.                          |
| `ERR_SECTION_WHITESPACE` | `'SECTION contains whitespace'`        | Die Sektion enthält Leerzeichen.                            |
| `ERR_KEY_WHITESPACE`  | `'KEY contains whitespace'`              | Der Schlüssel enthält Leerzeichen.                          |
| `ERR_READ_ONLY`       | `'Key is read-only'`                     | Der Schlüssel ist schreibgeschützt und kann nicht geändert werden. |

Diese Konstanten werden typischerweise als Rückgabewerte von Funktionsblöcken verwendet, die auf INI- oder NVS-Speicher zugreifen.

## Technische Besonderheiten

- Die Konstanten sind im Paket `logiBUS::storage::const` definiert und können über den vollqualifizierten Namen (z. B. `logiBUS::storage::const::storageStatusMessages.OK`) angesprochen werden.  
- Sie sind als `VAR_GLOBAL CONSTANT` deklariert, d. h. ihre Werte sind zur Laufzeit unveränderlich.  
- Der Baustein wird in der 4diac-IDE als `GlobalConstants`-Typ geführt und kann in Projekten als Referenz auf das Konstantenset eingebunden werden.  
- Die Werte sind bewusst als `STRING` gehalten, um eine flexible Ausgabe und Verarbeitung in Anwendungen zu ermöglichen.

## Zustandsübersicht

Da es sich um einen Konstantenblock handelt, existieren keine internen Zustände oder Zustandsübergänge. Die Werte sind statisch und immer verfügbar.

## Anwendungsszenarien

- **Fehlerbehandlung in Speicherbausteinen**: Funktionsblöcke, die auf NVS oder INI zugreifen, verwenden diese Konstanten, um Statusmeldungen an übergeordnete Logik zu übergeben.  
- **Einheitliche Log-Ausgaben**: Die Meldungen können direkt für Diagnose- und Loggingzwecke genutzt werden, da sie sprechende Texte enthalten.  
- **Initialisierungskontrolle**: Mit `INITIALISED` und `NOT_INITIALISED` kann der Initialisierungsstatus eines Speicherbereichs geprüft werden.  
- **Schreibschutzprüfung**: Der Wert `ERR_READ_ONLY` hilft, Versuche auf schreibgeschützte Schlüssel zu erkennen.

## Vergleich mit ähnlichen Bausteinen

In 4diac existieren oft separate konstante Definitionen innerhalb einzelner Funktionsblöcke. Dieser GlobalConstants-Baustein bietet jedoch eine zentrale, wiederverwendbare Sammlung, die eine konsistente Fehler- und Statusbehandlung über viele Speicherfunktionen hinweg ermöglicht. Andere Bausteine könnten eigene Statuscodes definieren, was zu Inkonsistenzen führen kann. `storageStatusMessages` standardisiert diese Codes und reduziert so den Wartungsaufwand.

## Fazit

Der GlobalConstants-Baustein `storageStatusMessages` stellt eine klar strukturierte und dokumentierte Sammlung von Statusmeldungen für Speicherzugriffe bereit. Durch die zentrale Definition wird die Anwendungsentwicklung vereinfacht und die Zuverlässigkeit der Fehlerbehandlung erhöht. Er ist ein unverzichtbarer Bestandteil für Projekte, die mit NVS- oder INI-Speicher arbeiten.

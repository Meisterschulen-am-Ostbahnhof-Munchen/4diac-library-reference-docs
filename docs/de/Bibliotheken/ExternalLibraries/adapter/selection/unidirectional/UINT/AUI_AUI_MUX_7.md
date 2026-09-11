# AUI_AUI_MUX_7

![AUI_AUI_MUX_7](./AUI_AUI_MUX_7.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **AUI_AUI_MUX_7** ist ein Multiplexer, der es ermöglicht, einen von sieben Eingangsadaptern (IN1 bis IN7) auf einen Ausgangsadapter (OUT) zu schalten. Die Auswahl des aktiven Eingangs erfolgt über den Indexadapter **K**. Der Baustein ist als generischer Typ definiert und wird für die Auswahl von AUI-Datenströmen verwendet. Der Adapter-Ausgang wird nur bei einer tatsächlichen Wertänderung aktualisiert, wobei das Ereignis **CNF** nur bei einer Änderung des Index oder des ausgewählten Eingangs ausgelöst wird.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Es sind keine Ereignis-Eingänge vorhanden.

### **Ereignis-Ausgänge**

| Name | Beschreibung |
|------|--------------|
| CNF | Bestätigung der gesetzten Indexauswahl (Confirmation of Set Index K) |

### **Daten-Eingänge**

Es sind keine direkten Dateneingänge vorhanden; die Eingänge erfolgen über die Adapter.

### **Daten-Ausgänge**

Es sind keine direkten Datenausgänge vorhanden; der Ausgang wird über den Adapter OUT realisiert.

### **Adapter**

*Sockets (Eingangsadapter):*

| Name | Typ | Beschreibung |
|------|-----|--------------|
| K    | AUI (unidirectional) | Index zur Auswahl eines der Eingänge IN1..IN7 (0..6) |
| IN1  | AUI (unidirectional) | Eingangswert 1, ausgewählt bei K = 0 |
| IN2  | AUI (unidirectional) | Eingangswert 2, ausgewählt bei K = 1 |
| IN3  | AUI (unidirectional) | Eingangswert 3, ausgewählt bei K = 2 |
| IN4  | AUI (unidirectional) | Eingangswert 4, ausgewählt bei K = 3 |
| IN5  | AUI (unidirectional) | Eingangswert 5, ausgewählt bei K = 4 |
| IN6  | AUI (unidirectional) | Eingangswert 6, ausgewählt bei K = 5 |
| IN7  | AUI (unidirectional) | Eingangswert 7, ausgewählt bei K = 6 |

*Plugs (Ausgangsadapter):*

| Name | Typ | Beschreibung |
|------|-----|--------------|
| OUT  | AUI (unidirectional) | Ausgang, der den aktuell ausgewählten Eingangswert liefert |

## Funktionsweise

Der Baustein überwacht den Indexadapter **K** sowie alle Eingangsadapter. Sobald sich der Wert von **K** oder der Wert des aktuell ausgewählten Eingangs (IN1..IN7) ändert, wird der Ausgang **OUT** aktualisiert. Das Ereignis **CNF** wird ausgelöst, um eine erfolgreiche Übernahme des neuen Werts zu bestätigen. Die Auswahl erfolgt gemäß dem Index **K** mit folgender Zuordnung:

* K = 0 → IN1
* K = 1 → IN2
* K = 2 → IN3
* K = 3 → IN4
* K = 4 → IN5
* K = 5 → IN6
* K = 6 → IN7

Wenn **K** außerhalb des gültigen Bereichs liegt (größer als 6), wird kein Eingang ausgewählt und der Ausgang bleibt unverändert. Die Aktualisierung des Ausgangs erfolgt nur bei tatsächlicher Wertänderung, was unnötige Ereignisse vermeidet.

## Technische Besonderheiten

* Der Baustein ist als **generischer Typ** (**GenericClassName** = `GEN_AUI_AUI_MUX`) implementiert, wodurch er für andere Anzahlen von Eingängen erweiterbar ist.
* Der Baustein verwendet ausschließlich Adapter für die Ein-/Ausgabe (unidirektionaler AUI-Typ). Dadurch werden die datenflussorientierten Verbindungen vereinfacht.
* Die Erkennung von Wertänderungen basiert auf einem Vergleich der aktuellen und vorherigen Werte; nur bei Änderung werden **OUT** und **CNF** aktualisiert.
* Es gibt keine ereignisgesteuerten Eingänge; die gesamte Logik wird durch Datenänderungen an den Adaptern getriggert.
* Der Baustein wurde für den Einsatz in der Agrartechnik entwickelt und unter der Eclipse Public License 2.0 veröffentlicht.

## Zustandsübersicht

Der Baustein besitzt keinen expliziten Zustandsautomaten. Die Funktionsweise ist rein reaktiv: Bei jeder Änderung an **K** oder dem ausgewählten Eingang wird der Ausgang neu berechnet und das Ereignis **CNF** ausgelöst, falls sich der Ausgangswert tatsächlich geändert hat. Es gibt keine internen Zustände außer den aktuell gespeicherten Werten.

## Anwendungsszenarien

* **Datenquellenumschaltung**: Der Baustein kann verwendet werden, um zwischen mehreren Sensoren oder Datenquellen umzuschalten, die über AUI-Schnittstellen angebunden sind.
* **Redundanzsteuerung**: In Systemen mit redundanten Pfaden kann der Multiplexer den aktiven Pfad basierend auf einem Steuersignal wählen.
* **Flexible Konfiguration**: Durch die Adapterstruktur lassen sich verschiedene AUI-basierte Komponenten dynamisch auswählen.

## Vergleich mit ähnlichen Bausteinen

Andere Multiplexer-Bausteine bieten oft einzelne Dateneingänge (z.B. INTEGER) anstelle von Adaptern. Der Vorteil dieses Bausteins liegt in der Verwendung von Adaptern, die komplexe Datentypen und ereignisbasierte Kommunikation integrieren können. Nachteil könnte die fixe Anzahl der Eingänge sein (hier 7). Im Vergleich zu einem generischen Multiplexer, der über Parameter konfigurierbar ist, ist dieser Baustein spezialisiert, aber durch die Generik erweiterbar.

## Fazit

Der **AUI_AUI_MUX_7** ist ein spezialisierter Multiplexer für AUI-basierte Verbindungen. Er ermöglicht eine saubere und effiziente Auswahl von bis zu sieben Eingangsadaptern auf einen Ausgang. Durch die ausschließliche Verwendung von Adaptern und die Ereignisausgabe bei Wertänderungen eignet er sich gut für Echtzeit- und Automatisierungssysteme, insbesondere in der Agrartechnik. Die generische Implementierung erlaubt eine einfache Anpassung an andere Eingangsanzahlen.

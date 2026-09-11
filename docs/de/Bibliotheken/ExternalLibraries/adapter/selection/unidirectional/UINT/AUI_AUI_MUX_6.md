# AUI_AUI_MUX_6

![AUI_AUI_MUX_6](./AUI_AUI_MUX_6.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `AUI_AUI_MUX_6` ist ein generischer Multiplexer, der über sechs unidirektionale AUI-Adapter (Application User Interface) Eingangswerte entgegennimmt und einen davon – basierend auf einem über einen weiteren AUI-Adapter bereitgestellten Index – an seinen Ausgang weiterleitet. Der Baustein ist speziell für die Auswahl eines von sechs Kanälen konzipiert und arbeitet ereignisgesteuert: Das Ausgangsereignis wird nur dann ausgelöst, wenn sich der am Ausgang anliegende Wert tatsächlich ändert. Dadurch werden unnötige Übertragungen und Prozesslast vermieden. Der FB ist als generischer Baustein mit dem Klassennamen `GEN_AUI_AUI_MUX` definiert und wird über Plug & Socket-Verbindungen in 4diac‑IDE integriert.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine Ereignis-Eingänge vorhanden.

### **Ereignis-Ausgänge**

| Name | Typ   | Kommentar                                       |
|------|-------|-------------------------------------------------|
| `CNF`| Event | Bestätigung des gesetzten Index (K) und der Aktualisierung des Ausgangs. |

### **Daten-Eingänge**

Keine Daten-Eingänge vorhanden.

### **Daten-Ausgänge**

Keine Daten-Ausgänge vorhanden.

### **Adapter**

Der FB verwendet ausschließlich unidirektionale AUI‑Adapter (Typ `adapter::types::unidirectional::AUI`) für den Datenaustausch.

**Ausgang (Plug)**

| Name | Typ (Adapter)          | Kommentar                                       |
|------|------------------------|-------------------------------------------------|
| `OUT`| `unidirectional::AUI`  | Ausgangswert, entspricht dem ausgewählten Eingang (IN1 – IN6). |

**Eingänge (Sockets)**

| Name | Typ (Adapter)          | Kommentar                                       |
|------|------------------------|-------------------------------------------------|
| `K`  | `unidirectional::AUI`  | Index-Steuerung: wählt den aktiven Eingang (Wert 0–5). |
| `IN1`| `unidirectional::AUI`  | Eingang 1 – aktiv bei K = 0                     |
| `IN2`| `unidirectional::AUI`  | Eingang 2 – aktiv bei K = 1                     |
| `IN3`| `unidirectional::AUI`  | Eingang 3 – aktiv bei K = 2                     |
| `IN4`| `unidirectional::AUI`  | Eingang 4 – aktiv bei K = 3                     |
| `IN5`| `unidirectional::AUI`  | Eingang 5 – aktiv bei K = 4                     |
| `IN6`| `unidirectional::AUI`  | Eingang 6 – aktiv bei K = 5                     |

## Funktionsweise

Der Baustein `AUI_AUI_MUX_6` implementiert eine 6‑zu‑1‑Auswahl (Multiplexer) auf Basis von AUI‑Adaptern. Der über den Adapter `K` übertragene Indexwert bestimmt, welcher der sechs Eingangsadapter (`IN1` bis `IN6`) aktuell durchgeschaltet wird. Der Wert des ausgewählten Eingangs wird permanent am Ausgangsadapter `OUT` bereitgestellt.

Wesentlich ist die ereignisgesteuerte Aktualisierung: Der Ausgang `OUT` wird nur dann mit einem neuen Wert versehen und das Ereignis `CNF` ausgelöst, wenn sich der Wert des gewählten Eingangs gegenüber dem zuletzt gesendeten Ausgangswert geändert hat. Dies gilt sowohl bei einem Wechsel des Index (K) als auch bei einer Änderung des aktiven Eingangswerts. Bleibt der Wert unverändert, wird kein Ereignis erzeugt.

Die Kommunikation erfolgt asynchron und unidirektional über die jeweiligen Adapter‑Schnittstellen. Der Baustein besitzt keine eigenen Daten‑ oder Ereignis‑Ein‑/Ausgänge, sondern ausschließlich Adapter, die eine lose Kopplung und Wiederverwendbarkeit ermöglichen.

## Technische Besonderheiten

- **Generischer Aufbau**: Der Baustein ist als generischer FB mit dem Klassennamen `GEN_AUI_AUI_MUX` definiert. Dadurch kann er parametrisiert und in unterschiedlichen Kontexten eingesetzt werden.
- **Nur Adapter‑Schnittstelle**: Sämtliche Ein‑ und Ausgänge sind als unidirektionale AUI‑Adapter ausgeführt. Dies erlaubt eine flexible Verbindung mit anderen AUI‑basierten Bausteinen und eine modulare Systemarchitektur.
- **Ereignis‑Minimierung**: Durch die interne Erkennung von Wertänderungen wird das Ereignis `CNF` nur bei tatsächlichen Änderungen generiert. Dies reduziert die Netzwerk- und Prozessorlast in verteilten Systemen.
- **Keine Zustandsautomaten**: Die Logik ist rein funktional; es existiert kein explizit modellierter Zustandsautomat. Die Auswahl und Aktualisierung erfolgt vollständig datengetrieben.
- **Lizenzierung**: Der Baustein ist unter der Eclipse Public License 2.0 veröffentlicht.

## Zustandsübersicht

Da der Baustein keinen internen Zustandsautomaten besitzt, entfällt eine klassische Zustandsdarstellung. Die Funktionsweise lässt sich vielmehr durch folgende logische Abläufe beschreiben:

1. **Initialisierung**: Beim ersten Durchlauf wird der Eingang `IN1` als Ausgang gesetzt, falls keine gültige Indexvorgabe vorliegt.
2. **Indexauswahl**: Durch Änderung des Adapters `K` wird der entsprechende Eingang aktiviert.
3. **Wertprüfung**: Der aktivierte Eingang wird kontinuierlich überwacht. Bei einer Wertänderung wird der neue Wert auf `OUT` gespiegelt und das Ereignis `CNF` erzeugt.
4. **Keine Änderung**: Liegt keine Änderung vor, bleibt der Ausgang unverändert und es wird kein Ereignis ausgelöst.

Somit existiert nur der funktionale Zustand „aktiv“ ohne spezifische Unterzustände.

## Anwendungsszenarien

- **Kanalumschaltung in Automatisierungssystemen**: Auswahl eines von sechs analogen oder digitalen Messwerten zur Weiterverarbeitung.
- **Redundante Datenquellen**: Umschaltung auf einen Ersatzeingang bei Ausfall des Primärsignals.
- **Konfigurierbare Datenweiterleitung**: In modularen Anlagen kann über den Indexadapter dynamisch festgelegt werden, welcher Datenstrom an den Ausgang gelangt.
- **Ereignisbasierte Kommunikation**: Aufgrund der Änderungserkennung eignet sich der FB besonders für Netzwerke mit begrenzter Bandbreite, da nur relevante Änderungen übertragen werden.

## Vergleich mit ähnlichen Bausteinen

Gegenüber klassischen Multiplexern, die über separate Daten‑Eingänge (z. B. INT, REAL) und einen Auswahl‑Datenwert verfügen, zeichnet sich `AUI_AUI_MUX_6` durch die Verwendung von Adapter‑Schnittstellen aus. Adapter ermöglichen eine semantische Bündelung von Daten und Ereignissen und vereinfachen die Verbindung zu komplexen Komponenten. Im Vergleich zu einer direkten Implementierung mit `MUX`‑Funktionsbausteinen (z. B. nach IEC 61499) bietet dieser FB eine höhere Abstraktionsebene und eine genauere Anpassung an AUI‑basierte Architekturen. Andere Multiplexer verfügen oft über feste Eingangsanzahlen oder erfordern zusätzliche Ereignis‑Steuerung; hier erfolgt die Auswahl rein über den Indexadapter ohne separate Enable‑Signale.

## Fazit

Der `AUI_AUI_MUX_6` ist ein flexibler und effizienter Multiplexer für die Auswahl aus sechs AUI‑Datenquellen. Durch die vollständige Adapter‑Schnittstelle und die ausschließlich ereignisgesteuerte Aktualisierung bei Wertänderungen bietet er eine optimale Lösung für verteilte Automatisierungssysteme, die Wert auf geringe Netzlast und modulare Wiederverwendbarkeit legen. Die generische Implementierung und die klare Definition als 6‑zu‑1‑Auswahl machen ihn zu einem nützlichen Baustein in vielfältigen Anwendungsumgebungen.

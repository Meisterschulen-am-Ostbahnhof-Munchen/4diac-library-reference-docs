# AUDI_AUI_MUX_7

![AUDI_AUI_MUX_7](./AUDI_AUI_MUX_7.svg)

* * * * * * * * * *
## Einleitung

Der Funktionsbaustein **AUDI_AUI_MUX_7** realisiert einen generischen Multiplexer, der über einen Index (K) einen von sieben Eingängen (IN1 bis IN7) auf einen Ausgang (OUT) durchschaltet. Der Baustein ist speziell für die Verwendung mit den Adaptertypen `adapter::types::unidirectional::AUDI` und `adapter::types::unidirectional::AUI` ausgelegt und arbeitet rein ereignisgesteuert. Der Ausgang wird dabei nur dann aktualisiert, wenn sich der tatsächliche Wert am ausgewählten Eingang ändert. Dies wird durch das Ereignis `CNF` bestätigt.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine – der Baustein besitzt keine Ereignis-Eingänge.

### **Ereignis-Ausgänge**

| Name  | Typ     | Kommentar |
|-------|---------|-----------|
| `CNF` | `Event` | Bestätigt, dass der Index K gesetzt und der Ausgang bei einer tatsächlichen Wertänderung aktualisiert wurde. |

### **Daten-Eingänge**

Keine direkten Dateneingänge; die Eingangsdaten werden ausschließlich über die Adapter-Schnittstellen bereitgestellt.

### **Daten-Ausgänge**

Keine direkten Datenausgänge; die Ausgangsdaten werden ausschließlich über die Adapter-Schnittstelle bereitgestellt.

### **Adapter**

| Richtung | Name | Typ                                  | Kommentar |
|----------|------|--------------------------------------|-----------|
| Ausgang (Plug) | `OUT` | `adapter::types::unidirectional::AUDI` | Ausgangswert, der dem ausgewählten Eingang entspricht. |
| Eingang (Socket) | `K`   | `adapter::types::unidirectional::AUI`  | Indexauswahl (0 bis 6) für die Eingangswahl. |
| Eingang (Socket) | `IN1` | `adapter::types::unidirectional::AUDI` | Eingangswert 1, ausgewählt bei K = 0. |
| Eingang (Socket) | `IN2` | `adapter::types::unidirectional::AUDI` | Eingangswert 2, ausgewählt bei K = 1. |
| Eingang (Socket) | `IN3` | `adapter::types::unidirectional::AUDI` | Eingangswert 3, ausgewählt bei K = 2. |
| Eingang (Socket) | `IN4` | `adapter::types::unidirectional::AUDI` | Eingangswert 4, ausgewählt bei K = 3. |
| Eingang (Socket) | `IN5` | `adapter::types::unidirectional::AUDI` | Eingangswert 5, ausgewählt bei K = 4. |
| Eingang (Socket) | `IN6` | `adapter::types::unidirectional::AUDI` | Eingangswert 6, ausgewählt bei K = 5. |
| Eingang (Socket) | `IN7` | `adapter::types::unidirectional::AUDI` | Eingangswert 7, ausgewählt bei K = 6. |

## Funktionsweise

Der Baustein arbeitet als Multiplexer mit sieben Datenquellen und einer Indexsteuerung. Der Wert am Adapter `K` wird als Index interpretiert. Abhängig vom Wert (0 für `IN1`, 1 für `IN2`, ... bis 6 für `IN7`) wird der korrespondierende Eingang ausgewählt. Der aktuelle Wert des ausgewählten Eingangs wird auf den Ausgang `OUT` übertragen.

Die Besonderheit liegt in der Ereignisbehandlung: Ein `CNF`-Ereignis wird nur dann erzeugt, wenn sich der Wert am Ausgang tatsächlich ändert. Dies reduziert unnötige Ereignisflüsse und verbessert die Effizienz in der Datenweiterleitung.

Der Baustein ist als generischer Funktionsbaustein (Generic FB) deklariert (`eclipse4diac::core::GenericClassName = 'GEN_AUDI_AUI_MUX'`). Dadurch kann er in unterschiedlichen Kontexten wiederverwendet werden, ohne die internen Typen fest zu verdrahten.

## Technische Besonderheiten

- **Generischer Typ:** Der Baustein ist als generischer FB implementiert, was eine flexible Anpassung an verschiedene Datentypen ermöglicht (hier speziell die Adaptertypen `AUDI` und `AUI`).
- **Ereignisreduzierung:** Der Ausgang wird nur bei einer Wertänderung aktualisiert; dies wird durch das Ereignis `CNF` bestätigt.
- **Adapterbasierte Kommunikation:** Alle Ein- und Ausgänge sind über unidirektionale Adapter (`AdapterDeclaration`) realisiert, was eine lose Kopplung und einfache Integration in bestehende Adaptertopologien ermöglicht.
- **Kein Zustandsautomat:** Es ist kein ECC (Execution Control Chart) definiert; die Verarbeitung erfolgt rein ereignisorientiert ohne interne Zustände.

## Zustandsübersicht

Da kein ECC vorhanden ist, existiert keine explizite Zustandsmaschine. Der Baustein führt bei Aktivierung die Multiplexfunktion deterministisch aus. Das `CNF`-Ereignis stellt das Abschlussereignis dar und signalisiert die erfolgreiche Verarbeitung des aktuellen Index und die Aktualisierung des Ausgangs (falls eine Wertänderung stattgefunden hat).

## Anwendungsszenarien

- **Datenwegebündelung:** In Systemen, in denen mehrere Sensoren oder Datenquellen über einen einzigen Kanal ausgelesen werden sollen, kann der Multiplexer verwendet werden, um sequenziell verschiedene Eingänge zu selektieren.
- **Konfigurierbare Signalauswahl:** In Steuerungen mit wechselnden Signalquellen (z. B. Messumformer, Diagnosekanäle) wird über den Index `K` dynamisch die gewünschte Quelle ausgewählt.
- **Energieeffiziente Kommunikation:** Durch die ereignisbasierte Aktualisierung nur bei Wertänderungen eignet sich der Baustein für Applikationen mit begrenzter Bandbreite oder bei denen unnötige Datenübertragungen vermieden werden sollen.

## Vergleich mit ähnlichen Bausteinen

Typische Multiplexer in der 4diac-Umgebung verwenden oft direkte Dateneingänge und einen numerischen Index. Der `AUDI_AUI_MUX_7` zeichnet sich durch die Adapter-basierte Schnittstelle und die ereignisreduzierte Ausgabe aus. Gegenüber einem klassischen Datenmultiplexer (z. B. `SEL` oder `MUX`) bietet er eine höhere Flexibilität in der Datenmodellierung über Adapter und minimiert die Ereignislast durch die Wertänderungserkennung. Andere Bausteine erzeugen bei jedem Indexwechsel ein Ereignis, auch wenn sich der Datenwert nicht ändert – hier wird dies gezielt vermieden.

## Fazit

Der **AUDI_AUI_MUX_7** ist ein leistungsfähiger, generischer Multiplexer für Adapter-basierte Kommunikation. Seine Fähigkeit, den Ausgang nur bei tatsächlichen Wertänderungen zu aktualisieren und dies über das Ereignis `CNF` zu bestätigen, macht ihn besonders für Anwendungen geeignet, bei denen Effizienz und Datenreduktion im Vordergrund stehen. Die klare Trennung von Eingängen, Ausgang und Indexsteuerung ermöglicht eine einfache Integration in bestehende Automatisierungsstrukturen und bietet eine flexible, wiederverwendbare Lösung für vielfältige Selektionsaufgaben.
# A2X_AUI_MUX_4

![A2X_AUI_MUX_4](./A2X_AUI_MUX_4.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **A2X_AUI_MUX_4** ist ein generischer 4-zu-1-Multiplexer auf Basis unidirektionaler IEC-61499-Adapter. Er wählt über den Indexadapter **K** genau einen der vier Eingangsadapter **IN1** bis **IN4** aus und stellt dessen Wert am Ausgangsadapter **OUT** bereit.

Der Baustein ist als generischer FB mit dem Klassennamen `GEN_A2X_AUI_MUX` angelegt. Er ist speziell für den Einsatz mit **A2X**-Adaptern vorgesehen und aktualisiert den Adapter-Ausgang nur bei tatsächlicher Wertänderung des ausgewählten Eingangs.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `CNF` | Event | Bestätigung, dass der über `K` ausgewählte Eingangswert auf `OUT` ausgegeben wurde. |

### **Daten-Eingänge**

Keine direkten Dateneingänge. Die Eingangsdaten werden ausschließlich über Adapter-Sockets übertragen.

### **Daten-Ausgänge**

Keine direkten Datenausgänge. Die Ausgangsdaten werden ausschließlich über den Adapter-Plug `OUT` bereitgestellt.

### **Adapter**

| Name | Richtung | Typ | Beschreibung |
|------|----------|-----|--------------|
| `OUT` | Plug | `adapter::types::unidirectional::A2X` | Ausgangsadapter mit dem gewählten Wert. |
| `K`   | Socket  | `adapter::types::unidirectional::AUI` | Indexadapter zur Auswahl des aktiven Eingangs. |
| `IN1` | Socket  | `adapter::types::unidirectional::A2X` | Eingangswert 1, aktiv für `K = 0`. |
| `IN2` | Socket  | `adapter::types::unidirectional::A2X` | Eingangswert 2, aktiv für `K = 1`. |
| `IN3` | Socket  | `adapter::types::unidirectional::A2X` | Eingangswert 3, aktiv für `K = 2`. |
| `IN4` | Socket  | `adapter::types::unidirectional::A2X` | Eingangswert 4, aktiv für `K = 3`. |

## Funktionsweise

Der Baustein arbeitet als adapterbasierter Multiplexer:

1. Der Index `K` legt fest, welcher der vier Eingänge ausgewählt wird.
2. Bei `K = 0` wird `IN1` ausgewählt, bei `K = 1` `IN2`, bei `K = 2` `IN3` und bei `K = 3` `IN4`.
3. Der Wert des ausgewählten Eingangsadapter wird auf den Ausgangsadapter `OUT` übertragen.
4. `OUT` wird nur dann aktualisiert, wenn sich der Wert gegenüber der letzten Ausgabe tatsächlich geändert hat.
5. Eine Wertänderung wird über das Ereignis `CNF` quittiert. Bleibt der Wert unverändert, wird kein Ereignis erzeugt.

## Technische Besonderheiten

- Der FB ist als generischer Baustein mit dem Attribut `eclipse4diac::core::GenericClassName` vom Typ `GEN_A2X_AUI_MUX` definiert.
- Die Schnittstellen bestehen vollständig aus unidirektionalen Adaptern der Typen `A2X` und `AUI`; es gibt keine klassischen Datenein- und -ausgänge.
- Die Ausgabe erfolgt ausschließlich bei Wertänderung. Dadurch wird unnötige Kommunikation auf dem Adapter-Ausgang vermieden.
- Die Versionsinformation weist darauf hin, dass das konkrete Adapter-Backend `GEN_A2X_AUI_MUX` noch zu implementieren ist.
- Der Baustein ist für die Paketstruktur `adapter::selection::unidirectional` vorgesehen.

## Zustandsübersicht

In der XML-Definition ist keine explizite ECC-Zustandsmaschine enthalten. Funktional lässt sich das Verhalten jedoch in folgende logische Zustände gliedern:

| Zustand | Beschreibung | Ausgangsverhalten |
|---------|--------------|-------------------|
| `Bereit` | Der FB wartet auf einen gültigen Index `K` und prüft die Eingänge. | Kein Ereignis. |
| `Vergleich` | Der Wert des ausgewählten Eingangs wird mit dem zuletzt ausgegebenen Wert verglichen. | Kein Ereignis. |
| `Aktualisierung` | Eine Wertänderung wurde erkannt; `OUT` wird gesetzt. | `CNF` wird erzeugt. |
| `Unverändert` | Der Wert entspricht dem letzten Ausgangswert. | Kein Ereignis, `OUT` bleibt unverändert. |

## Anwendungsszenarien

- **Signalumschaltung**: Auswahl einer von vier A2X-Datenquellen, z. B. unterschiedliche Sensoren, auf einen gemeinsamen Ausgang.
- **Redundanzumschaltung**: Umschalten zwischen mehreren redundanten Datenpfaden, ohne permanente Ausgangsereignisse zu erzeugen.
- **Parametrierbare Datenauswahl**: Nutzung eines AUI-Indexes zur dynamischen Auswahl eines Datenstroms in einer Produktions- oder Steuerungsanlage.
- **Generischer Einsatz**: Durch die generische Klassenbindung kann der FB für verschiedene Datenformate verwendet werden, sobald das Backend `GEN_A2X_AUI_MUX` implementiert ist.

## Vergleich mit ähnlichen Bausteinen

Gegenüber einem klassischen Daten-MUX besitzt `A2X_AUI_MUX_4` keine direkten Eingangs- und Ausgangsdaten, sondern nutzt ausschließlich unidirektionale Adapter. Dadurch können auch komplexe oder typisierte Datenstrukturen übertragen werden.

Im Unterschied zu einer einfachen `AX_AUI_MUX_4`-Variante ist dieser Baustein speziell für **A2X**-Adapter angepasst. Zusätzlich wird der Ausgang nur bei tatsächlicher Änderung aktualisiert, wodurch Ereignis- und Kommunikationsaufwand reduziert werden.

## Fazit

`A2X_AUI_MUX_4` ist ein flexibler, adapterbasierter 4-zu-1-Multiplexer für die IEC-61499-Umgebung. Er reduziert durch die Wertänderungserkennung unnötige Ausgangsereignisse und eignet sich besonders für die dynamische Auswahl von A2X-Datenströmen. Vor dem produktiven Einsatz muss das zugehörige generische Backend `GEN_A2X_AUI_MUX` implementiert werden.

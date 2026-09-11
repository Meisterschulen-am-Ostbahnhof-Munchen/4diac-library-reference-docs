# A2X_AUI_MUX_5

![A2X_AUI_MUX_5](./A2X_AUI_MUX_5.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **A2X_AUI_MUX_5** ist ein generischer Multiplexer auf Basis unidirektionaler Adapter. Er wählt anhand eines über den **AUI**-Adapter zugeführten Indexes einen von fünf **A2X**-Adaptereingängen aus und leitet dessen Wert über einen **A2X**-Adapterausgang weiter. Der Ausgang wird dabei nur bei einer tatsächlichen Wertänderung aktualisiert; auch das Ereignis `CNF` wird ausschließlich bei einer solchen Änderung ausgelöst.

## Schnittstellenstruktur

Der Baustein besitzt keine klassischen Datenein- oder Datenausgänge. Alle Werte werden über Adapter übertragen. Ereignisse stehen nur als Ereignisausgänge zur Verfügung.

### **Ereignis-Eingänge**

Es sind keine Ereignis-Eingänge definiert.

### **Ereignis-Ausgänge**

| Ereignis | Kommentar |
|---|---|
| `CNF` | Bestätigung der Übernahme des über `K` ausgewählten Indexes (`Confirmation of Set Index K`) |

### **Daten-Eingänge**

Es sind keine Daten-Eingänge vorhanden. Die Eingabedaten werden ausschließlich über die Adapter `IN1` bis `IN5` bereitgestellt.

### **Daten-Ausgänge**

Es sind keine Daten-Ausgänge vorhanden. Der Ausgabewert wird ausschließlich über den Adapter `OUT` bereitgestellt.

### **Adapter**

| Adapter | Richtung | Typ | Kommentar |
|---|---|---|---|
| `OUT` | Ausgang (Plug) | `adapter::types::unidirectional::A2X` | Ausgangswert; entspricht dem über `K` ausgewählten Eingang |
| `K` | Eingang (Socket) | `adapter::types::unidirectional::AUI` | Indexauswahl: `0` bis `4` |
| `IN1` | Eingang (Socket) | `adapter::types::unidirectional::A2X` | Eingangswert 1, ausgewählt bei `K = 0` |
| `IN2` | Eingang (Socket) | `adapter::types::unidirectional::A2X` | Eingangswert 2, ausgewählt bei `K = 1` |
| `IN3` | Eingang (Socket) | `adapter::types::unidirectional::A2X` | Eingangswert 3, ausgewählt bei `K = 2` |
| `IN4` | Eingang (Socket) | `adapter::types::unidirectional::A2X` | Eingangswert 4, ausgewählt bei `K = 3` |
| `IN5` | Eingang (Socket) | `adapter::types::unidirectional::A2X` | Eingangswert 5, ausgewählt bei `K = 4` |

## Funktionsweise

Der Baustein arbeitet als adapterbasierter Multiplexer:

1. Der `K`-Adapter liefert einen Indexwert im Bereich `0` bis `4`.
2. Anhand dieses Indexes wird einer der fünf A2X-Eingänge ausgewählt:

   - `K = 0` → `IN1`
   - `K = 1` → `IN2`
   - `K = 2` → `IN3`
   - `K = 3` → `IN4`
   - `K = 4` → `IN5`

3. Der Wert des ausgewählten Eingangs wird auf den `OUT`-Adapter übertragen.
4. Die Übertragung auf `OUT` erfolgt nur dann, wenn sich der resultierende Wert gegenüber dem zuletzt gesendeten Wert tatsächlich geändert hat.
5. Bei einer tatsächlichen Wertänderung wird zusätzlich das Ereignis `CNF` ausgelöst. Bleibt der Wert unverändert, wird kein Ereignis erzeugt.

Der Baustein reagiert damit nicht nur auf eine Änderung des Indexes, sondern auch auf eine Wertänderung am aktuell ausgewählten Eingang.

## Technische Besonderheiten

- Der Baustein ist als **generischer Funktionsbaustein** ausgelegt. Über das Attribut `eclipse4diac::core::GenericClassName` wird die generische Implementierungsklasse `GEN_A2X_AUI_MUX` referenziert.
- Die Datenübertragung erfolgt vollständig über **unidirektionale Adapter** vom Typ `A2X` und `AUI`. Es sind keine separaten Datenein- oder Datenausgänge vorhanden.
- Die Ausgangsaktualisierung erfolgt **ereignisgesteuert bei Wertänderung**. Dadurch werden unnötige Aktualisierungen am Adapterausgang vermieden.
- Der zulässige Indexbereich ist durch die Adapterkommentare mit `0` bis `4` festgelegt; für jeden Index ist genau einer der fünf Eingänge vorgesehen.
- In der vorliegenden Version ist die generische Backend-Implementierung `GEN_A2X_AUI_MUX` als noch zu implementieren gekennzeichnet. Für den Einsatz des Bausteins muss dieses Backend in der Zielumgebung verfügbar sein.

## Zustandsübersicht

Ein explizites Zustandsdiagramm ist in der XML nicht hinterlegt. Logisch lässt sich das Verhalten des Bausteins in zwei Zustände unterteilen:

| Zustand | Beschreibung |
|---|---|
| `WARTEN` | Der Baustein wartet auf eine Änderung des Indexes `K` oder auf eine Wertänderung des aktuell ausgewählten Eingangs. |
| `AKTUALISIEREN` | Bei einer tatsächlichen Wertänderung wird der neue Wert auf `OUT` übertragen und das Ereignis `CNF` ausgelöst. |

Ein Wechsel in den Zustand `AKTUALISIEREN` findet nur statt, wenn sich der auszugebende Wert tatsächlich ändert. Andernfalls verbleibt der Baustein im Zustand `WARTEN`.

## Anwendungsszenarien

- **Adapterbasierte Signalumschaltung:** Auswahl eines von fünf analogen oder digitalen A2X-Signalen über einen zentralen Indexadapter.
- **Modulare Maschinensteuerung:** Dynamisches Durchschalten verschiedener Sensor- oder Steuerwerte an einen gemeinsamen Ausgangsadapter.
- **Redundante Wertauswahl:** Umschaltung zwischen mehreren Datenquellen, ohne dass die angeschlossenen Bausteine explizite Ereigniseingänge bereitstellen müssen.
- **Vermeidung unnötiger Ausgangsaktivität:** Durch die Aktualisierung nur bei Wertänderung bleiben nachgeschaltete Bausteine weitgehend ruhig, solange sich der ausgewählte Wert nicht ändert.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Eigenschaft |
|---|---|
| `A2X_AUI_MUX_5` | Adapterbasierter Multiplexer mit unidirektionalen `A2X`- und `AUI`-Adaptern; Ausgang nur bei Wertänderung aktiv. |
| `AX_AUI_MUX_5` | Verwandte Variante ohne die spezielle `A2X`-Adapteranbindung. `A2X_AUI_MUX_5` stellt eine darauf aufbauende Variante dar. |
| Konventioneller MUX-Funktionsbaustein | Klassische Multiplexer verwenden in der Regel diskrete Dateneingänge und Ereigniseingänge. Der vorliegende Baustein nutzt dagegen ausschließlich Adapter zur Datenübertragung. |

## Fazit

`A2X_AUI_MUX_5` ist ein kompakter, adapterbasierter Multiplexer für bis zu fünf A2X-Eingänge. Durch die Auswahl über einen AUI-Index und die Aktualisierung nur bei tatsächlicher Wertänderung eignet er sich besonders für modulare, ereignisarme Adapterverbindungen in IEC-61499-Systemen. Der generische Aufbau ermöglicht eine flexible Wiederverwendung, sobald das zugehörige Backend `GEN_A2X_AUI_MUX` verfügbar ist.

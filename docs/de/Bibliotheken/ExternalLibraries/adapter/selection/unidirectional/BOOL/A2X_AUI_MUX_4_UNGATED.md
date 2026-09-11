# A2X_AUI_MUX_4_UNGATED

![A2X_AUI_MUX_4_UNGATED](./A2X_AUI_MUX_4_UNGATED.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **A2X_AUI_MUX_4_UNGATED** ist ein 4-zu-1-Multiplexer auf Adapterbasis. Er wählt über den als Index dienenden **AUI**-Adapter **K** einen von vier **A2X**-Eingängen aus und leitet dessen Daten beziehungsweise Ereignisse an den Ausgangs-Adapter **OUT** weiter.

Die Bezeichnung **UNGATED** kennzeichnet eine Variante ohne Änderungserkennung: Im Gegensatz zu einem gated Multiplexer wird hier **jedes neu berechnete Ergebnis** bedingungslos weitergegeben. Das ist für Verbraucher gedacht, die eine periodische Kadenz benötigen, unabhängig davon, ob sich ein Wert tatsächlich geändert hat – beispielsweise bei Ableitungs- oder Frequenzberechnungen.

* * * * * * * * * *

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Baustein besitzt keine direkten Ereignis-Eingänge. Die auslösenden Ereignisse werden über die Adapter-Sockets transportiert.

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `CNF` | Event | Bestätigung, dass der Index `K` gesetzt und die entsprechende Auswahl übernommen wurde. |

### **Daten-Eingänge**

Es sind keine direkten Daten-Eingänge vorhanden. Die Nutzdaten werden über die **A2X**-Adapter-Sockets `IN1` bis `IN4` sowie über den **AUI**-Adapter `K` eingelesen.

### **Daten-Ausgänge**

Es sind keine direkten Daten-Ausgänge vorhanden. Der ausgewählte Datenstrom wird über den **A2X**-Adapter-Plug `OUT` ausgegeben.

### **Adapter**

| Name | Richtung | Typ | Bedeutung |
|------|----------|-----|-----------|
| `K` | Socket | `adapter::types::unidirectional::AUI` | Auswahlindex. |
| `IN1` | Socket | `adapter::types::unidirectional::A2X` | Eingangswert 1, ausgewählt bei `K = 0`. |
| `IN2` | Socket | `adapter::types::unidirectional::A2X` | Eingangswert 2, ausgewählt bei `K = 1`. |
| `IN3` | Socket | `adapter::types::unidirectional::A2X` | Eingangswert 3, ausgewählt bei `K = 2`. |
| `IN4` | Socket | `adapter::types::unidirectional::A2X` | Eingangswert 4, ausgewählt bei `K = 3`. |
| `OUT` | Plug | `adapter::types::unidirectional::A2X` | Ausgang mit dem ausgewählten A2X-Datenstrom. |

Alle Adapter sind unidirektional. Der Datenfluss verläuft von den Sockets `IN1`–`IN4` und `K` zum Plug `OUT`.

* * * * * * * * * *

## Funktionsweise

Der Baustein verhält sich wie ein klassischer 4-zu-1-Multiplexer:

1. Der **AUI**-Socket `K` liefert einen Indexwert.
2. Anhand dieses Indexes wird einer der vier **A2X**-Sockets `IN1`–`IN4` ausgewählt.
3. Das über diesen Socket ankommende Adapter-Paket wird unverändert an den **A2X**-Plug `OUT` weitergegeben.
4. Anschließend wird das Ereignis `CNF` ausgelöst.

Die entscheidende Eigenschaft ist das **Fehlen einer Änderungserkennung**. Der Baustein vergleicht keinen alten mit einem neuen Wert. Jedes ankommende Ereignis und jedes neu berechnete Ergebnis wird sofort und ohne Prüfung weitergereicht. Dadurch bleibt auch dann ein kontinuierlicher Datenstrom erhalten, wenn sich der Wert über mehrere Zyklen nicht ändert.

* * * * * * * * * *

## Technische Besonderheiten

- **Keine Änderungserkennung:** Der Baustein unterdrückt keine unveränderten Werte. Jedes Ereignis wird durchgeschaltet.
- **Adapterbasierte Ein-/Ausgabe:** Es gibt keine klassischen Datenein- oder Datenausgänge; alle Werte laufen über die A2X- beziehungsweise AUI-Adapter.
- **Generischer Baustein:** Die tatsächliche Implementierung wird über das Attribut `eclipse4diac::core::GenericClassName` mit `GEN_A2X_AUI_MUX` referenziert.
- **Unidirektionale Adapter:** Der Baustein verwendet ausschließlich unidirektionale Adapter, was eine klare, gerichtete Datenflussstruktur ermöglicht.
- **Indexbereich:** Der Auswahlindex `K` wird entsprechend den Kommentaren als Wert `0` bis `3` erwartet. Werte außerhalb dieses Bereichs sind nicht definiert.
- **Direkte Bestätigung:** Der Ereignisausgang `CNF` signalisiert, dass der Index `K` übernommen wurde.

* * * * * * * * * *

## Zustandsübersicht

Der Baustein ist als generischer Funktionsblock ohne eigenes, in der XML abgebildetes ECC definiert. Konzeptionell lassen sich folgende Phasen beschreiben:

| Zustand | Beschreibung |
|---------|--------------|
| **Bereit** | Der Baustein wartet auf einen gültigen Index über den AUI-Adapter `K`. |
| **Auswahl** | Der Index wird ausgewertet und der zugehörige Eingang `IN1`–`IN4` ausgewählt. |
| **Durchreichen** | Das Datenpaket des gewählten Eingangs wird unabhängig von einer Wertänderung an `OUT` weitergegeben. |
| **Bestätigen** | Das Ereignis `CNF` wird ausgelöst; der Baustein kehrt danach in den Bereit-Zustand zurück. |

Es gibt keinen Zustand, der eine Weitergabe aufgrund unveränderter Werte blockiert.

* * * * * * * * * *

## Anwendungsszenarien

- **Ableitungsberechnung:** Für die Bildung eines Differenzenquotienten muss jeder Abtastwert verarbeitet werden, auch wenn der Messwert konstant bleibt. Nur so kann eine saubere Ableitung von Null taktrichtig ausgegeben werden.
- **Frequenz- und Periodendauerermittlung:** Verbraucher benötigen eine lückenlose Ereignisfolge, um Zeitintervalle zuverlässig zu messen.
- **Datenquellen-Umschaltung:** In einer Regelung oder Visualisierung können verschiedene A2X-Datenquellen über den Index `K` umgeschaltet werden, ohne dass die Ausgabe bei gleichen Werten verzögert wird.
- **Test- und Prüfumgebungen:** Jede Berechnung eines vorgelagerten Bausteins soll protokolliert oder weiterverarbeitet werden, unabhängig davon, ob sich der Wert geändert hat.

* * * * * * * * * *

## Vergleich mit ähnlichen Bausteinen

| Baustein | Verhalten | Geeignet für |
|----------|-----------|--------------|
| `A2X_AUI_MUX_4` | Multiplexer mit Änderungserkennung; leitet nur bei Wertänderung weiter. | Verbraucher, die ausschließlich auf geänderte Werte reagieren sollen. |
| `A2X_AUI_MUX_4_UNGATED` | Multiplexer ohne Änderungserkennung; leitet jedes Ereignis bedingungslos weiter. | Zeitkontinuierliche Auswertungen, Ableitungen, Frequenzberechnungen, lückenlose Protokollierung. |

Die grundlegende Multiplexer-Funktion ist identisch. Der einzige wesentliche Unterschied liegt in der Filterung durch die Änderungserkennung.

* * * * * * * * * *

## Fazit

Der Funktionsblock **A2X_AUI_MUX_4_UNGATED** ist eine sinnvolle Ergänzung zur variantenreichen A2X-Multiplexer-Familie. Er bietet eine einfache, adapterbasierte 4-zu-1-Auswahl und verzichtet bewusst auf jede Änderungserkennung. Dadurch eignet er sich besonders für Anwendungen, die eine periodische, lückenlose und ungefilterte Weitergabe von berechneten Ergebnissen benötigen.

# A2X_AUI_MUX_2

![A2X_AUI_MUX_2](./A2X_AUI_MUX_2.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **A2X_AUI_MUX_2** ist ein generischer 2‑zu‑1‑Multiplexer auf Basis von Adapter‑Schnittstellen. Er wählt zwischen zwei A2X‑Adapter‑Eingängen anhand eines über einen AUI‑Adapter bereitgestellten Index aus und leitet den gewählten Wert an den A2X‑Adapter‑Ausgang weiter. Eine Besonderheit ist die aktualisierungsoptimierte Arbeitsweise: Der Ausgang wird nur dann mit einem neuen Wert versorgt, wenn sich der ausgewählte Wert tatsächlich ändert. Ebenso wird das Bestätigungsereignis **CNF** ausschließlich bei einer realen Wertänderung ausgelöst.

Der Baustein ist als generischer Funktionsbaustein (Generic FB) angelegt. Die eigentliche Ausführungslogik wird über die zugeordnete generische Klasse `GEN_A2X_AUI_MUX` bereitgestellt und kann an die konkreten Adapter‑Typen angepasst werden.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

| Name | Typ   | Kommentar                                        |
|------|-------|--------------------------------------------------|
| CNF  | Event | Bestätigung der Übernahme des gewählten Index K. |

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

| Name | Richtung | Typ                                                | Kommentar                                               |
|------|----------|----------------------------------------------------|---------------------------------------------------------|
| K    | Socket   | `adapter::types::unidirectional::AUI`              | Index zur Auswahl des Eingangs.                         |
| IN1  | Socket   | `adapter::types::unidirectional::A2X`              | Eingangswert 1, aktiv wenn K = 0.                       |
| IN2  | Socket   | `adapter::types::unidirectional::A2X`              | Eingangswert 2, aktiv wenn K = 1.                       |
| OUT  | Plug     | `adapter::types::unidirectional::A2X`              | Ausgang, liefert den ausgewählten Wert.                 |

## Funktionsweise

Der Baustein überwacht den über den Adapter **K** bereitgestellten Index. Der Indexwert bestimmt, welcher der beiden A2X‑Eingänge zur Auswahl kommt:

- Bei **K = 0** wird der Wert von **IN1** zum Ausgang **OUT** durchgeschaltet.
- Bei **K = 1** wird der Wert von **IN2** zum Ausgang **OUT** durchgeschaltet.

Sobald sich entweder der Index K oder der aktuell ausgewählte Eingangswert ändert, wird der neue Wert mit dem zuletzt am Ausgang anliegenden Wert verglichen. Nur wenn sich der Wert tatsächlich unterscheidet, wird der Ausgangs‑Adapter **OUT** mit dem neuen Wert aktualisiert und anschließend das Ereignis **CNF** ausgelöst. Bleibt der Wert unverändert, unterbleibt sowohl die Aktualisierung des Ausgangs als auch die Ereignisausgabe. Dadurch werden unnötige Datenübertragungen und Ereignisfluten im nachgelagerten Netzwerk vermieden.

## Technische Besonderheiten

- **Generischer Funktionsbaustein:** Die konkrete Verarbeitungslogik wird über die generische Klasse `GEN_A2X_AUI_MUX` bereitgestellt. Die Implementierung ist noch nicht vollständig im Standardumfang enthalten und muss für die jeweilige Zielumgebung bereitgestellt werden.
- **Änderungsbasierte Ausgabe:** Der Ausgang wird nur bei einer tatsächlichen Wertänderung aktualisiert. Dadurch wird das nachgeschaltete System entlastet und die Datenkonsistenz bei unveränderten Werten sichergestellt.
- **Ereignis nur bei Änderung:** Das Bestätigungsereignis **CNF** wird ausschließlich dann gesendet, wenn der neue Wert vom vorherigen Ausgangswert abweicht.
- **Adapterbasierte Schnittstellen:** Sämtliche Ein- und Ausgänge sind als unidirektionale Adapter ausgeführt. Dadurch ist der Baustein flexibel in verschiedene Adapter‑Topologien integrierbar.
- **Zwei Eingänge, ein Index:** Die Auswahl erfolgt ausschließlich über den adapterbasierten Index K. Die Wertebereiche der Adapter‑Typen A2X und AUI werden durch die jeweiligen Adapterdefinitionen festgelegt.

## Zustandsübersicht

Der Baustein besitzt keine explizit modellierten Zustände. Die Funktionsweise kann jedoch als impliziter Ablauf beschrieben werden:

1. **Warten:** Der Baustein wartet auf Änderungen am Index K oder an den Eingangsadaptern IN1/IN2.
2. **Auswahl:** Der aktuelle Index K wird ausgewertet und der entsprechende Eingang ausgewählt.
3. **Vergleich:** Der neue Wert wird mit dem aktuell am Ausgang anliegenden Wert verglichen.
4. **Aktualisierung & Bestätigung:** Bei Abweichung wird OUT aktualisiert und CNF ausgelöst. Bei Übereinstimmung bleibt der Zustand unverändert.

## Anwendungsszenarien

- **Datenweiche:** Umschaltung zwischen zwei Datenquellen anhand eines externen Steuersignals, beispielsweise zur redundanten Signalzuführung.
- **Sensorumschaltung:** Auswahl zwischen zwei gleichartigen Sensoren, wobei nur eine Änderung des tatsächlich aktiven Sensors weitergereicht wird.
- **Effiziente Weiterleitung:** Einsatz in verzweigten Kommunikationsstrukturen, in denen unnötige Aktualisierungen vermieden werden sollen, etwa bei kontinuierlich gleichbleibenden Prozesswerten.
- **Generische Adapter‑Integration:** Verwendung in Systemen, die auf die Adapter‑Typen AUI und A2X aufbauen und einen austauschbaren Multiplexer benötigen.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu einfachen Multiplexern, die bei jeder Änderung eines Eingangs oder des Index ein Ereignis auslösen, arbeitet **A2X_AUI_MUX_2** ereignisoptimiert. Ein herkömmlicher Multiplexer würde den Ausgang bei jeder Neuberechnung aktualisieren, auch wenn sich der Wert nicht ändert. Dieser Baustein unterdrückt solche redundanten Ausgaben und erzeugt nur dann ein **CNF**‑Ereignis, wenn der tatsächliche Wert am Ausgang wechselt.

Gegenüber einer nicht‑generischen Variante ist dieser Baustein über die generische Klasse `GEN_A2X_AUI_MUX` an verschiedene Adapterausprägungen anpassbar. Dadurch ist er flexibler, erfordert jedoch eine angepasste Implementierung in der Zielumgebung.

## Fazit

Der **A2X_AUI_MUX_2** ist ein zweikanaliger, adapterbasierter Multiplexer mit dem Fokus auf effizienter und ereignisreduzierter Werteweitergabe. Durch die Auswahl über den AUI‑Adapter und die Ausgabe über einen A2X‑Adapter fügt er sich nahtlos in adapterorientierte 4diac‑Lösungen ein. Die Kombination aus generischer Struktur und änderungsbasierter Aktualisierung macht ihn besonders geeignet für Systeme, bei denen Kommunikationsaufkommen und Signalaktualität sorgfältig balanciert werden müssen.

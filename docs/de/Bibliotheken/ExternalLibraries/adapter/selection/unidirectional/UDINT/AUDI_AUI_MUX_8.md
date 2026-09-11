# AUDI_AUI_MUX_8

![AUDI_AUI_MUX_8](./AUDI_AUI_MUX_8.svg)

* * * * * * * * * *
## Einleitung

Der Funktionsblock `AUDI_AUI_MUX_8` ist ein generischer Multiplexer, der über einen Adapter-Index `K` einen von acht Eingangswerten (`IN1` bis `IN8`) auswählt und diesen über den Adapter-Ausgang `OUT` bereitstellt. Der Baustein wurde speziell für die effiziente Weitergabe von Datenströmen entworfen und aktualisiert den Ausgang nur bei einer tatsächlichen Wertänderung. Dadurch werden unnötige Ereignisse vermieden und die Netzwerklast reduziert.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
- Keine vorhanden.

### **Ereignis-Ausgänge**
- **CNF** (Confirmation): Wird ausgelöst, wenn sich der ausgewählte Wert (also der aktuelle Wert am Ausgang) tatsächlich ändert. Dies kann durch eine Änderung des Index `K` oder durch eine neue Wertänderung des aktuell ausgewählten Eingangs verursacht werden.

### **Daten-Eingänge**
- **K** (Socket, Typ `AUI`): Index zur Auswahl des aktiven Eingangs. Gültige Werte sind 0 bis 7.
- **IN1** (Socket, Typ `AUDI`): Eingangswert 1, aktiv bei `K = 0`.
- **IN2** (Socket, Typ `AUDI`): Eingangswert 2, aktiv bei `K = 1`.
- **IN3** (Socket, Typ `AUDI`): Eingangswert 3, aktiv bei `K = 2`.
- **IN4** (Socket, Typ `AUDI`): Eingangswert 4, aktiv bei `K = 3`.
- **IN5** (Socket, Typ `AUDI`): Eingangswert 5, aktiv bei `K = 4`.
- **IN6** (Socket, Typ `AUDI`): Eingangswert 6, aktiv bei `K = 5`.
- **IN7** (Socket, Typ `AUDI`): Eingangswert 7, aktiv bei `K = 6`.
- **IN8** (Socket, Typ `AUDI`): Eingangswert 8, aktiv bei `K = 7`.

### **Daten-Ausgänge**
- **OUT** (Plug, Typ `AUDI`): Gibt den Wert des aktuell ausgewählten Eingangs weiter.

### **Adapter**
- **Ausgangsadapter (Plug):** `OUT`
- **Eingangsadapter (Sockets):** `K`, `IN1` – `IN8`

## Funktionsweise

Der Baustein wartet auf Aktualisierungen der angeschlossenen Adapter. Sobald sich entweder der Index `K` oder der Wert des momentan ausgewählten Eingangs ändert, wird der neue Wert an den Ausgang `OUT` übertragen und das Ereignis `CNF` ausgelöst. Diese Logik stellt sicher, dass keine unnötigen Ereignisse generiert werden, wenn sich nicht relevante Eingänge ändern oder wenn sich der Index zwar ändert, aber der ausgewählte Wert identisch bleibt (z. B. wenn zwei Eingänge denselben Wert liefern). Der Baustein ist als generischer Funktionsblock definiert, wodurch er mit verschiedenen Datentypen wiederverwendet werden kann, die den jeweiligen Adapterdefinitionen entsprechen.

## Technische Besonderheiten

- **Generische Implementierung:** Der FB verwendet das Attribut `GenericClassName = 'GEN_AUDI_AUI_MUX'`, was eine typlose, wiederverwendbare Ausführung ermöglicht.
- **Adapter-basierte Schnittstelle:** Alle Werte werden über Adapter übertragen, die eine lose Kopplung zwischen den Bausteinen in einem verteilten System ermöglichen.
- **Effiziente Ereignissteuerung:** Nur bei tatsächlicher Wertänderung wird ein Ereignis `CNF` ausgegeben, was die Netzwerkkommunikation reduziert.
- **Keine Ereignis-Eingänge:** Die Steuerung des Bausteins erfolgt rein über die Datenaktualisierung der Adapter; ein explizites Trigger-Ereignis ist nicht erforderlich.

## Zustandsübersicht

Der FB besitzt keine extern sichtbaren Zustände im klassischen Sinne. Im internen Verhalten wird jedoch implizit der aktuelle Index `K` und der zuletzt ausgegebene Wert gespeichert, um eine Änderung zu erkennen. Diese internen Zustände sind:

- **Wartezustand:** Es wird auf eine Änderung des Index oder des ausgewählten Eingangswerts gewartet.
- **Aktualisierungszustand:** Bei Erkennung einer relevanten Änderung wird der Ausgangswert übernommen und `CNF` ausgelöst, danach wechselt der FB zurück in den Wartezustand.

## Anwendungsszenarien

- **Datenauswahl in Messsystemen:** Auswahl eines von mehreren Sensoren (z. B. Temperatur, Druck) auf Grundlage eines Steuerindex.
- **Umschaltung von Kommunikationskanälen:** Flexibles Routing von Datenpaketen zwischen verschiedenen Quellen und einem Ziel.
- **Parametrierung:** Dynamisches Einspielen von Konfigurationswerten aus mehreren Quellen in eine Steuerung.
- **Redundanzverwaltung:** Umschalten auf redundante Eingänge bei Ausfall eines Sensors (durch externe Logik, die den Index anpasst).

## Vergleich mit ähnlichen Bausteinen

Gegenüber klassischen MUX-Bausteinen, die über separate Daten- und Ereignis-Eingänge verfügen, zeichnet sich dieser FB durch seine Adapter-Schnittstelle und die ereignisoptimierte Ausgabe aus. Konventionelle Multiplexer geben oft bei jeder Indexänderung ein Ereignis aus, unabhängig davon, ob sich der Wert wirklich ändert. `AUDI_AUI_MUX_8` eliminiert unnötige Events und ist dadurch für datengetriebene industrielle Anwendungen besser geeignet. Der Verzicht auf Ereignis-Eingänge vereinfacht die Einbindung in Systeme, in denen Datenwerte kontinuierlich aktualisiert werden.

## Fazit

Der `AUDI_AUI_MUX_8` ist ein leistungsfähiger und flexibler Multiplexer-Baustein, der durch seine Adapter-basierte Architektur und die intelligente Ereignissteuerung besonders für moderne, verteilte Automatisierungssysteme geeignet ist. Er reduziert unnötige Kommunikation und ermöglicht eine klare, modulare Struktur bei der Auswahl und Weitergabe von Datenwerten. Mit seiner generischen Natur lässt er sich vielfältig in verschiedenen Anwendungskontexten einsetzen.
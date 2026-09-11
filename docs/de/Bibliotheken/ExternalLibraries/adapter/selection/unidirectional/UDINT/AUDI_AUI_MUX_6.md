# AUDI_AUI_MUX_6

![AUDI_AUI_MUX_6](./AUDI_AUI_MUX_6.svg)

* * * * * * * * * *
## Einleitung
Der Funktionsbaustein **AUDI_AUI_MUX_6** realisiert einen generischen Multiplexer, der aus sechs unidirektionalen Adaptereingängen (IN1 bis IN6) den aktuell ausgewählten Wert über den Adapterausgang OUT bereitstellt. Die Auswahl des aktiven Eingangs erfolgt über einen separaten Indexadapter K, der Werte von 0 bis 5 annimmt. Ein Ereignis CNF wird nur dann ausgelöst, wenn sich der Ausgangswert tatsächlich ändert, wodurch unnötige Ereignisse vermieden werden.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
Keine Ereignis-Eingänge vorhanden.

### **Ereignis-Ausgänge**
| Ereignis | Beschreibung |
|----------|--------------|
| `CNF`    | Bestätigung der Übernahme des neuen Index und damit verbundener Änderung des Ausgangswerts (nur bei tatsächlicher Wertänderung). |

### **Daten-Eingänge**
Keine direkten Dateneingänge definiert. Die Daten werden ausschließlich über die Adaptereingänge übertragen.

### **Daten-Ausgänge**
Keine direkten Datenausgänge definiert. Die Daten werden ausschließlich über den Adapterausgang bereitgestellt.

### **Adapter**
| Adapter | Typ | Richtung | Beschreibung |
|---------|-----|----------|--------------|
| `OUT`   | `adapter::types::unidirectional::AUDI` | Plug | Ausgangsadapter, stellt den ausgewählten Eingangswert bereit. |
| `K`     | `adapter::types::unidirectional::AUI` | Socket | Indexauswahl (0–5). |
| `IN1`   | `adapter::types::unidirectional::AUDI` | Socket | Eingangswert 1 (wird bei K=0 ausgewählt). |
| `IN2`   | `adapter::types::unidirectional::AUDI` | Socket | Eingangswert 2 (K=1). |
| `IN3`   | `adapter::types::unidirectional::AUDI` | Socket | Eingangswert 3 (K=2). |
| `IN4`   | `adapter::types::unidirectional::AUDI` | Socket | Eingangswert 4 (K=3). |
| `IN5`   | `adapter::types::unidirectional::AUDI` | Socket | Eingangswert 5 (K=4). |
| `IN6`   | `adapter::types::unidirectional::AUDI` | Socket | Eingangswert 6 (K=5). |

## Funktionsweise
Der Baustein arbeitet als Adapter-basierter Multiplexer: Der über den Socket `K` ankommende Indexwert bestimmt, welcher der sechs Eingangsadapter (`IN1`–`IN6`) an den Ausgangsadapter `OUT` weitergereicht wird. Die Adapter sind unidirektional, d.h. die Datenübertragung erfolgt von den Sockets zum Plug.

Bei jeder Änderung des Indexwerts `K` wird der entsprechende Eingang ausgewählt. Der Ausgangswert wird nur dann aktualisiert, wenn sich der vom neuen Eingang gelieferte Wert vom vorherigen Ausgangswert unterscheidet. In diesem Fall wird das Ereignis `CNF` ausgelöst. Bei gleichen Werten bleibt der Ausgang unverändert und es wird kein `CNF` gesendet.

## Technische Besonderheiten
- **Generische Implementierung:** Der Baustein wird als generischer FB (GenericClassName `'GEN_AUDI_AUI_MUX'`) bereitgestellt, was eine flexible Nutzung in verschiedenen Kontexten ermöglicht.
- **Adapterbasiert:** Die Kommunikation erfolgt ausschließlich über Adapter, wodurch eine klare und lose Kopplung ermöglicht wird.
- **Unidirektionale Adapter:** Sowohl die Eingangs- als auch die Ausgangsadapter sind unidirektional, d.h. es werden nur Daten vom Socket zum Plug übertragen.
- **Schwellwert-Triggerung:** Das Ereignis `CNF` wird nur bei tatsächlicher Wertänderung erzeugt, was die Ereignislast im System reduziert.
- **Sechs Eingänge:** Bietet die Auswahl aus sechs unterschiedlichen Datenquellen, wobei der Indexbereich 0–5 abgedeckt wird.

## Zustandsübersicht
Der FB besitzt keinen expliziten Zustandsautomaten, dennoch lässt sich das Verhalten in folgende Phasen strukturieren:

1. **Warten auf Indexänderung:** Der FB verharrt in einem Ruhezustand, bis der Wert am Adapter `K` einen neuen gültigen Index (0–5) annimmt.
2. **Auswahl und Prüfung:** Der neue Index wird verarbeitet, der entsprechende Eingang wird an den Ausgang gekoppelt. Der FB vergleicht den neuen Wert mit dem zuletzt gesendeten Ausgangswert.
3. **Aktualisierung (optional):** Wenn sich der Wert geändert hat, wird der Ausgangsadapter aktualisiert und das Ereignis `CNF` gesendet. Bleibt der Wert gleich, wird kein Ereignis erzeugt und der FB kehrt in den Wartezustand zurück.

## Anwendungsszenarien
- **Multiplexen analoger Messwerte:** Auswahl eines von sechs Sensoren (z.B. Temperatur, Druck, Feuchte) für die Weiterverarbeitung in einem Automatisierungssystem.
- **Kanalumschaltung:** Dynamische Umschaltung zwischen mehreren Datenquellen (z.B. in der Agrartechnik für die Auswahl verschiedener Betriebsmodi).
- **Reduzierung der Ereignislast:** Einsatz in Systemen, in denen unnötige Aktualisierungen vermieden werden sollen, da der Baustein nur bei echten Wertänderungen Ereignisse erzeugt.

## Vergleich mit ähnlichen Bausteinen
Im Vergleich zu klassischen Multiplexern, die direkt über Dateneingänge arbeiten, nutzt dieser FB ausschließlich Adapter, was die Einbindung in adapterbasierte Architekturen erleichtert. Die Eigenschaft, nur bei Wertänderungen ein Ereignis auszulösen, unterscheidet ihn von einfachen Multiplexern, die bei jeder Indexänderung ein Ereignis senden. Dadurch wird die Kommunikation effizienter, insbesondere bei häufig wechselnden Indizes, aber selten wechselnden Datenwerten.

## Fazit
Der **AUDI_AUI_MUX_6** stellt einen flexiblen und effizienten Multiplexer für adapterbasierte Systeme dar. Durch seine generische Struktur und die intelligente Triggerung nur bei tatsächlichen Änderungen ist er ideal für Anwendungen, in denen mehrere Datenquellen verwaltet und unnötige Ereignisflut vermieden werden muss. Die klare Trennung über Adapter erleichtert die Wiederverwendung und Integration in bestehende 4diac-Architekturen.
# AUI_MUX_3

![AUI_MUX_3](./AUI_MUX_3.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AUI_MUX_3** implementiert einen Ereignis-Multiplexer mit drei Eingängen. Er stellt eine Erweiterung des Standardbausteins `E_MUX_3` dar, bei dem der selektierte Ereignisindex nicht über einen separaten Datenausgang `K`, sondern über einen unidirektionalen Adapter (Typ `adapter::types::unidirectional::AUI`) bereitgestellt wird. Dadurch kann das Ergebnis direkt an andere Bausteine mit kompatibler Adapterschnittstelle weitergegeben werden, ohne zusätzliche Verbindungen über Datenleitungen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **EI1** (Event): Ereignis, das den Index 0 auswählt.
- **EI2** (Event): Ereignis, das den Index 1 auswählt.
- **EI3** (Event): Ereignis, das den Index 2 auswählt.

### **Ereignis-Ausgänge**

- Keine expliziten Ereignis-Ausgänge vorhanden. Das Ergebnis wird über den Adapter `K` kommuniziert.

### **Daten-Eingänge**

- Keine.

### **Daten-Ausgänge**

- Keine direkten Datenausgänge. Der ausgewählte Index wird über den Adapter `K` übertragen.

### **Adapter**

- **K** (Plug, Typ `adapter::types::unidirectional::AUI`): Unidirektionaler Adapterausgang, der den Index (0, 1 oder 2) des zuletzt empfangenen Ereignisses als Datenwert bereitstellt. Die genaue Kodierung ist abhängig von der Definition des Adaptertyps.

## Funktionsweise

Der Baustein wartet auf Ereignisse an den Eingängen `EI1`, `EI2` oder `EI3`. Sobald eines dieser Ereignisse eintritt, wird der zugehörige Index (0, 1 bzw. 2) intern gespeichert und über den Adapterausgang `K` ausgegeben. Der Adapter übernimmt dabei die Rolle des sonst üblichen Datenausgangs `K` sowie des Ereignisausgangs `EO` in einem einzigen Interface. Dadurch wird die Weitergabe des selektierten Ereignisses an nachgeschaltete Bausteine vereinfacht, die eine einheitliche Adapterschnittstelle verwenden.

Der Baustein ist als generischer Funktionsblock (GenericClassName `'GEN_E_MUX'`) konzipiert und kann daher in unterschiedlichen Umgebungen eingesetzt werden.

## Technische Besonderheiten

- **Adapterbasierte Ausgabe**: Statt getrennter Event- und Datenausgänge wird ein unidirektionaler Adapter verwendet. Dies ermöglicht eine kompakte und standardisierte Anbindung an andere Bausteine mit demselben Adaptertyp.
- **Generischer Aufbau**: Durch die Zuweisung der generischen Klasse `GEN_E_MUX` ist der Baustein für verschiedene Konfigurationen wiederverwendbar.
- **Keine Datenabhängigkeit**: Die Auswahl des Index erfolgt rein über Ereignis-Eingänge; es werden keine zusätzlichen Datenwerte benötigt.
- **Kompatibilität**: Der Adaptertyp `adapter::types::unidirectional::AUI` ist Teil des Pakets `adapter::events::unidirectional` und muss in der Zielumgebung verfügbar sein.

## Zustandsübersicht

Der Baustein besitzt keinen expliziten Zustandsautomaten. Intern wird lediglich der zuletzt empfangene Index gespeichert und über den Adapter ausgegeben. Es gibt keine kontinuierliche Verarbeitung; die Ausgabe wird ausschließlich durch Ereignisse am Eingang aktualisiert.

## Anwendungsszenarien

- **Ereignis-Routing**: Auswahl einer von drei Quellen oder Pfaden basierend auf einem eingehenden Ereignis.
- **Prioritätssteuerung**: Zuordnung einer Prioritätsstufe (0, 1, 2) zu einem Ereignis.
- **Protokollumsetzung**: Einsatz in Systemen, bei denen eine Adapterschnittstelle für die Kommunikation zwischen Bausteinen verwendet wird, um eine saubere Trennung von Daten- und Steuerebene zu erreichen.
- **Modulare Automatisierungslösungen**: Anbindung an einen übergeordneten Auswahlbaustein über einen standardisierten Adapter.

## Vergleich mit ähnlichen Bausteinen

Der Standardbaustein **E_MUX_3** besitzt neben den drei Ereignis-Eingängen einen Ereignis-Ausgang `EO` und einen Datenausgang `K` (Integer), die separat verdrahtet werden müssen. **AUI_MUX_3** ersetzt diese beiden Ausgänge durch einen einzelnen Adapter `K`. Dadurch werden Verbindungen vereinfacht und die Schnittstelle wird einheitlicher, insbesondere wenn mehrere Bausteine über denselben Adaptertyp kommunizieren sollen. Die interne Funktionalität – Auswahl und Bereitstellung des Index – ist identisch, lediglich die Ausgabeschnittstelle unterscheidet sich.

## Fazit

Der Funktionsblock **AUI_MUX_3** ist eine moderne, adapterbasierte Variante des klassischen Ereignis-Multiplexers. Er bietet eine kompakte und flexible Möglichkeit, Ereignisse zu selektieren und das Ergebnis über eine standardisierte Schnittstelle weiterzugeben. Insbesondere für Systeme, die auf Adapterkommunikation setzen, stellt er eine sinnvolle Erweiterung dar und erleichtert die Verschaltung von Automatisierungskomponenten.

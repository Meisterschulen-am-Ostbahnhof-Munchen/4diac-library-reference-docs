# AUI_DEMUX_7

![AUI_DEMUX_7](./AUI_DEMUX_7.svg)

* * * * * * * * * *
## Einleitung

Der AUI_DEMUX_7 ist ein generischer Event-Demultiplexer mit sieben Ausgängen. Er leitet ein eingehendes Ereignis (EI) abhängig von einem über einen AUI-Adapter bereitgestellten Index (K) an einen der sieben Ereignisausgänge weiter. Dieser Baustein stellt eine Alternative zum klassischen E_DEMUX_7 dar, bei dem der Index über einen einfachen Dateneingang übergeben wird, während hier eine adapterbasierte Verbindung verwendet wird.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
Es ist kein Ereignis-Eingang in der XML-Schnittstelle explizit deklariert. Als generischer Funktionsbaustein (GenericClassName: `GEN_E_DEMUX`) besitzt der Baustein jedoch einen impliziten Ereigniseingang `EI`, der bei der Instanziierung ergänzt wird. Dieser Eingang nimmt das zu multiplexende Ereignis entgegen.

### **Ereignis-Ausgänge**
- **EO1** – Ausgang für Index 0
- **EO2** – Ausgang für Index 1
- **EO3** – Ausgang für Index 2
- **EO4** – Ausgang für Index 3
- **EO5** – Ausgang für Index 4
- **EO6** – Ausgang für Index 5
- **EO7** – Ausgang für Index 6

Die Kommentare in der XML geben an, dass jeder Ausgang das demultiplexte Signal von EI abhängig vom Wert von K liefert.

### **Daten-Eingänge**
Keine Daten-Eingänge vorhanden. Die Auswahl des Zielausgangs erfolgt ausschließlich über den Adapter.

### **Daten-Ausgänge**
Keine Daten-Ausgänge vorhanden.

### **Adapter**
- **K** (Socket) – Typ: `adapter::types::unidirectional::AUI`  
  Dient zur Übergabe des Indexwertes (0–6), der bestimmt, welcher Ereignisausgang aktiviert wird.

## Funktionsweise

Ein am Eingang `EI` ankommendes Ereignis wird über den Demultiplexer an den Ausgang weitergeleitet, dessen Index dem aktuellen Wert des Adapters `K` entspricht. Wenn der Wert von `K` im Bereich 0 bis 6 liegt, wird genau einer der Ausgänge `EO1` bis `EO7` getriggert. Bei Werten außerhalb dieses Bereichs wird kein Ausgang aktiviert, das Ereignis wird verworfen.

Da der Baustein generisch ist, kann der Typ des Eingangsereignisses und der anderen Parameter über die generische Instanziierung angepasst werden. Der konkrete Ereigniseingang `EI` wird durch die zugrunde liegende generische Klasse `GEN_E_DEMUX` ergänzt.

## Technische Besonderheiten

- **Generischer Baustein:** Durch das Attribut `eclipse4diac::core::GenericClassName` mit dem Wert `'GEN_E_DEMUX'` wird der Baustein als generischer Funktionsbaustein deklariert. Das bedeutet, dass der Funktionsumfang und die Schnittstelle teilweise zur Laufzeit definiert werden.
- **Adapterbasierte Indexübergabe:** Der Index wird nicht über einen einfachen Dateneingang, sondern über einen Socket vom Typ `AUI` (unidirectional) übergeben. Dies ermöglicht eine modulare und lose gekoppelte Verbindung zur übergeordneten Logik.
- **Anzahl der Ausgänge:** Sieben Ausgänge sind fest im Typ definiert; eine dynamische Änderung der Ausgangsanzahl ist nicht vorgesehen.

## Zustandsübersicht

Es existiert kein expliziter Zustandsautomat. Die Funktionsweise ist rein ereignisgetrieben: Beim Eintreffen eines Ereignisses am Eingang `EI` wird sofort der passende Ausgang gemäß dem aktuellen Adapterwert `K` aktiviert.

## Anwendungsszenarien

- Verteilung von Ereignissen in einer Steuerungsanwendung auf verschiedene Verarbeitungseinheiten (z. B. je nach ausgewähltem Modus).
- Umschaltung zwischen verschiedenen Betriebsmodi über einen Adapter-basierten Auswahlmechanismus.
- Einsatz in modularen Systemen, in denen die Auswahl logik über eine Adapter-Schnittstelle erfolgen soll.

## Vergleich mit ähnlichen Bausteinen

Der klassische `E_DEMUX_7` besitzt einen direkten Ereigniseingang `EI` und einen Dateneingang `K` (typischerweise als INTEGER). Der `AUI_DEMUX_7` ersetzt den Dateneingang durch einen Adapter-Socket. Dies erhöht die Flexibilität bei der Anbindung, da der Index nicht als separate Datenleitung, sondern über eine komplexere Adapterstruktur übergeben wird. Dadurch ist eine klarere Trennung von Steuerlogik und Ereignisweiterleitung möglich. Als generischer Baustein kann er zudem in unterschiedlichen Kontexten mit variierenden Ereignistypen verwendet werden.

## Fazit

Der `AUI_DEMUX_7` ist ein nützlicher generischer Event-Demultiplexer, der sich durch die Verwendung eines AUI-Adapters für die Indexübergabe von herkömmlichen Demultiplexern abhebt. Er eignet sich besonders für modulare Steuerungsarchitekturen, in denen eine lose Kopplung und flexible Konfiguration gewünscht ist. Die feste Anzahl von sieben Ausgängen deckt typische Anwendungen ab, bei denen eine Auswahl aus mehreren Optionen getroffen werden muss.
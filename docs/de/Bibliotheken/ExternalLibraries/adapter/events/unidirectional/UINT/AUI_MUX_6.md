# AUI_MUX_6

![AUI_MUX_6](./AUI_MUX_6.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **AUI_MUX_6** ist ein Ereignis-Multiplexer, der sechs unabhängige Ereignis-Eingänge besitzt und über einen **AUI-Adapter** (unidirektional) das empfangene Ereignis zusammen mit dem zugehörigen Index (0–5) als Ausgabe bereitstellt. Er stellt eine generische Variante des klassischen E_MUX_6 dar, bei dem der konventionelle Ereignis-Ausgang und der Datenausgang für den Index durch einen einzigen Adapter ersetzt werden. Dadurch wird die Schnittstelle vereinheitlicht und die Weiterverarbeitung in IEC-61499-Systemen erleichtert.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **EI1** (Event): Ereignis, das auf Kanal 0 gemultiplext wird.
- **EI2** (Event): Ereignis, das auf Kanal 1 gemultiplext wird.
- **EI3** (Event): Ereignis, das auf Kanal 2 gemultiplext wird.
- **EI4** (Event): Ereignis, das auf Kanal 3 gemultiplext wird.
- **EI5** (Event): Ereignis, das auf Kanal 4 gemultiplext wird.
- **EI6** (Event): Ereignis, das auf Kanal 5 gemultiplext wird.

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

Keine vorhanden.

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

- **K** (Plug, Typ `adapter::types::unidirectional::AUI`): Unidirektionaler Adapter, der das empfangene Ereignis sowie den dazugehörigen Index (0–5) als gebündelte Schnittstelle nach außen führt.

## Funktionsweise

Der FB arbeitet als ereignisgesteuerter Multiplexer. Trifft ein Ereignis an einem der sechs Eingänge **EI1** bis **EI6** ein, so wird dieses Ereignis über den Adapter **K** ausgegeben. Gleichzeitig wird der entsprechende Index (0 für EI1, 1 für EI2, …, 5 für EI6) über denselben Adapter übermittelt. Dadurch kann der angeschlossene Empfänger sowohl das Ereignis als auch die Information, welcher Eingang das Ereignis ausgelöst hat, in einem Schritt verarbeiten. Es findet keine Priorisierung oder Verzögerung statt; jede Aktivierung eines Eingangs erzeugt sofort eine Ausgabe am Adapter.

## Technische Besonderheiten

- **Generische Implementierung**: Der Baustein ist als generische Klasse (`GenericClassName = 'GEN_E_MUX'`) definiert und kann für eine beliebige Anzahl von Eingängen instanziiert werden. Die vorliegende Instanz ist auf 6 Eingänge festgelegt.
- **Adapter-basierte Schnittstelle**: Der Einsatz eines Adapters anstelle separater Ereignis- und Datenausgänge vereinfacht die Verdrahtung in Systemen, die bereits Adapter für die Kommunikation verwenden.
- **Unidirektionaler Adapter**: Der Typ AUI ist als unidirektional definiert, d.h. er überträgt nur in eine Richtung (vom FB zum Empfänger).
- **Keine Zustandshaltung**: Der FB ist zustandslos – er speichert keine internen Werte und reagiert rein ereignisgetrieben.

## Zustandsübersicht

Da es sich um einen reinen Ereignis-Multiplexer ohne Zustandsspeicher handelt, existiert keine interne Zustandsmaschine. Der FB wechselt zwischen keinen Zuständen und führt bei jedem Ereignis am Eingang sofort die Multiplex-Funktion aus.

## Anwendungsszenarien

- **Zusammenführung von Ereignissen**: Mehrere Quellen (z. B. Sensoren, Alarme) erzeugen Ereignisse, die auf einem gemeinsamen Kommunikationspfad übertragen werden sollen – der Adapter bündelt Ereignis und Quellenidentifikation.
- **Steuerung von Ablaufsteuerungen**: In Fertigungsanlagen können verschiedene Maschinenzustände als Ereignisse gesammelt und an eine übergeordnete Steuerung weitergeleitet werden.
- **Datenvorverarbeitung**: Der Baustein kann als Eingangspuffer für zentrale Verarbeitungseinheiten dienen, die mehrere Ereigniskanäle mit einer einheitlichen Schnittstelle ansprechen möchten.

## Vergleich mit ähnlichen Bausteinen

Klassische Vertreter wie der **E_MUX_6** besitzen einen Ereignis-Ausgang (EO) und einen Datenausgang (K) für den Index. Der **AUI_MUX_6** ersetzt diese beiden Ausgänge durch einen einzigen AUI-Adapter, der beide Informationen zusammenführt. Vorteile:

- Reduzierung der Verbindungsleitungen,
- einheitliche Schnittstellenbeschreibung, 
- bessere Kompatibilität mit adapterbasierten Kommunikationsmustern in IEC 61499.

Nachteile gegenüber dem klassischen Baustein könnten darin liegen, dass der Empfänger zwingend einen passenden Adapter aufweisen muss, was bei einfachen Systemen mit getrennten Signalen unnötig wäre.

## Fazit

**AUI_MUX_6** stellt eine moderne, adapterbasierte Lösung für die Multiplexierung von Ereignissen dar. Durch die Bündelung von Ereignis und Index in einer einzigen Schnittstelle wird die Systemarchitektur vereinfacht und die Wiederverwendbarkeit erhöht. Die generische Natur des Bausteins ermöglicht eine flexible Anpassung an unterschiedliche Anforderungen, während die zustandslose Arbeitsweise eine präzise und zeitnahe Reaktion auf externe Ereignisse gewährleistet. Damit ist er eine wertvolle Komponente in ereignisgesteuerten Automatisierungssystemen.

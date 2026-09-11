# AUI_DEMUX_6

![AUI_DEMUX_6](./AUI_DEMUX_6.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AUI_DEMUX_6** ist ein Ereignis-Demultiplexer mit sechs Ausgängen. Er stellt eine spezielle Variante des generischen Bausteins `GEN_E_DEMUX` dar, bei dem der übliche separate Ereignis-Eingang (EI) zusammen mit dem Daten-Eingang (K) durch einen einzigen AUI-Adapter (Socket) ersetzt wurde. Über diesen Adapter wird ein Ereignis sowie eine Indexinformation übertragen, anhand derer das Ereignis an genau einen der sechs Ausgänge weitergeleitet wird.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Funktionsblock besitzt **keine direkten Ereignis-Eingänge**. Das Eingangsereignis wird stattdessen über den Adapter `K` empfangen. Der Adapter trägt hierbei sowohl das eigentliche Ereignissignal als auch den zugehörigen Indexwert.

### **Ereignis-Ausgänge**

- **EO1** – Ereignisausgang, aktiv wenn der empfangene Index = 0 ist  
- **EO2** – Ereignisausgang, aktiv wenn der empfangene Index = 1 ist  
- **EO3** – Ereignisausgang, aktiv wenn der empfangene Index = 2 ist  
- **EO4** – Ereignisausgang, aktiv wenn der empfangene Index = 3 ist  
- **EO5** – Ereignisausgang, aktiv wenn der empfangene Index = 4 ist  
- **EO6** – Ereignisausgang, aktiv wenn der empfangene Index = 5 ist  

### **Daten-Eingänge**

Es sind keine separate Daten-Eingänge vorhanden. Alle benötigten Daten (insbesondere der Indexwert) werden über den Adapter bereitgestellt.

### **Daten-Ausgänge**

Keine Daten-Ausgänge spezifiziert.

### **Adapter**

- **K** (Socket) – Typ: `adapter::types::unidirectional::AUI`  
  Über diesen Adapter empfängt der Baustein das Ereignis und den zugehörigen Indexwert. Der Adapter ist als unidirektionale Schnittstelle ausgelegt und stellt die Verbindung zu einem entsprechenden Plug eines anderen Bausteins her.

## Funktionsweise

Der Baustein wartet auf ein Ereignis, das über den Adapter `K` eintrifft. Zusammen mit diesem Ereignis wird ein ganzzahliger Indexwert (im Bereich 0 bis 5) über denselben Adapter übertragen. Sobald ein Ereignis empfangen wird, bestimmt der FB anhand des Indexwerts, welcher der sechs Ausgänge `EO1` bis `EO6` aktiviert wird. Das Ereignis wird also dem entsprechenden Ausgang zugeordnet und dort als Ausgangsereignis weitergegeben.

Der Ablauf ist rein ereignisgesteuert und besitzt keine internen Zustände. Die Zuordnung ist deterministisch: Index 0 → `EO1`, Index 1 → `EO2`, … Index 5 → `EO6`.

## Technische Besonderheiten

- **Adapterbasierte Eingangsschnittstelle:** Durch die Verwendung des AUI-Adapters wird der klassische Aufbau (separater Ereignis-Eingang und Daten-Eingang) zu einer einzigen, sauber gekapselten Schnittstelle zusammengefasst.
- **Generischer Baustein:** Über das Attribut `eclipse4diac::core::GenericClassName` wird der FB als spezielle Implementierung des generischen Typs `GEN_E_DEMUX` gekennzeichnet. Dadurch kann er in generischen Anwendungen verwendet werden.
- **Unidirektionale Kommunikation:** Der Adapter ist unidirektional ausgelegt, d.h. die Datenübertragung erfolgt ausschließlich vom Sender zum Empfänger.
- **Keine Datenhaltung:** Der Baustein ist stateless und benötigt keine internen Variablen, was eine einfache und robuste Wiederverwendung ermöglicht.

## Zustandsübersicht

Da der Funktionsblock keine internen Zustände besitzt, existiert keine Zustandsmaschine. Die Funktionalität beschränkt sich auf eine reine Ereignis-zu-Ereignis-Umschaltung, die ausschließlich vom aktuellen Indexwert abhängt.

## Anwendungsszenarien

- **Ereignisverteilung in Automatisierungssystemen:** Wenn in einer Steuerung ein Ereignis (z.B. ein Alarmsignal) in Abhängigkeit eines Prioritäts- oder Klassencodes an verschiedene Verarbeitungszweige weitergeleitet werden soll.
- **Modulare Signalweiche:** Einsatz in Systemen, die über Adapter kommunizieren, um lose gekoppelte Komponenten zu verbinden und gleichzeitig die Anzahl der Leitungen zu reduzieren.
- **Basis für eigene Demultiplexer-Erweiterungen:** Der Baustein kann als Referenz für ähnliche Adapter-basierte Varianten dienen, z.B. für eine andere Anzahl von Ausgängen oder andere Adaptertypen.

## Vergleich mit ähnlichen Bausteinen

Der Standard-Funktionsblock `E_DEMUX` verwendet einen separaten Ereignis-Eingang (`EI`) und einen Daten-Eingang (`K`) zur Auswahl des Ausgangs. Der `AUI_DEMUX_6` ersetzt diese beiden Eingänge durch einen einzigen AUI-Adapter, der sowohl das Ereignis als auch den Index überträgt.

| Eigenschaft | E_DEMUX | AUI_DEMUX_6 |
|-------------|---------|-------------|
| Ereignis-Eingang | Ja (EI) | Nein, über Adapter |
| Daten-Eingang | Ja (K) | Nein, über Adapter |
| Kommunikationsart | Direkt verdrahtet | Über Adapter (AUI) |
| Modularität | geringer | höher (via Plug/Socket) |
| Verdrahtungsaufwand | höher | geringer |

Der Vorteil der Adapter-Variante liegt in der verbesserten Modularität und Wiederverwendbarkeit. Der Nachteil besteht darin, dass der empfangende Baustein an eine spezifische Adapter-Schnittstelle gebunden ist, was die Kompatibilität mit anderen Bausteinen einschränken kann.

## Fazit

Der Funktionsblock `AUI_DEMUX_6` ist eine moderne, adapterbasierte Realisierung eines Ereignis-Demultiplexers für sechs Ausgänge. Er vereinfacht die Verkabelung, erhöht die Modularität und eignet sich besonders für Systeme, die auf der 4diac-Adaptertechnik basieren. Durch seine generische Natur lässt er sich leicht an unterschiedliche Anforderungen anpassen und stellt eine nützliche Erweiterung der Standard-Demultiplexer dar.

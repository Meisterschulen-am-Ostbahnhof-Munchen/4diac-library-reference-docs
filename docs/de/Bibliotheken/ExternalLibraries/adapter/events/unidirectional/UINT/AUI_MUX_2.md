# AUI_MUX_2

![AUI_MUX_2](./AUI_MUX_2.svg)

* * * * * * * * * *

## Einleitung

Der **AUI_MUX_2** ist ein ereignisgesteuerter Multiplexer-Baustein, der zwei Ereignis-Eingänge (EI1, EI2) über einen **AUI-Adapter** (Typ `adapter::types::unidirectional::AUI`) als Ausgangsschnittstelle bereitstellt. Er stellt eine spezielle Variante des klassischen `E_MUX_2` dar, bei dem der übliche Ereignis-Ausgang (EO) und der Daten-Eingang (K) durch einen einzigen unidirektionalen Adapter ersetzt wurden. Dadurch wird die Schnittstellenanzahl reduziert und eine nahtlose Integration in adapterbasierte Kommunikationsstrukturen ermöglicht.

Der Baustein ist als generischer Funktionsblock gekennzeichnet (Attribut `GenericClassName` = `'GEN_E_MUX'`) und eignet sich für Anwendungen, bei denen zwei verschiedene Ereignisquellen auf eine gemeinsame, über einen Adapter angebundene Verarbeitungseinheit geführt werden sollen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Datentyp | Kommentar |
|------|----------|-----------|
| EI1  | Event    | Ereignis 1, das über den Adapter weitergegeben wird |
| EI2  | Event    | Ereignis 2, das über den Adapter weitergegeben wird |

### **Ereignis-Ausgänge**

Der Baustein besitzt **keine** klassischen Ereignis-Ausgänge. Die Ausgabe erfolgt ausschließlich über den AUI-Adapter (siehe Abschnitt **Adapter**).

### **Daten-Eingänge**

Keine vorhanden.

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

| Name | Typ                                 | Richtung | Kommentar                      |
|------|-------------------------------------|----------|--------------------------------|
| K    | `adapter::types::unidirectional::AUI` | Ausgang  | Überträgt das gewählte/weitergeleitete Ereignis an den angeschlossenen Partner |

Der Adapter `K` stellt die einzige Ausgabeschnittstelle dar. Er ist als unidirektionaler Adapter ausgelegt und sendet bei jedem eintreffenden Ereignis an EI1 oder EI2 ein entsprechendes Ereignis an die angeschlossene Komponente.

## Funktionsweise

Der **AUI_MUX_2** arbeitet rein ereignisgesteuert:

- Wird an **EI1** ein Ereignis empfangen, wird dieses unmittelbar über den Adapter `K` nach außen gesendet.
- Wird an **EI2** ein Ereignis empfangen, wird ebenso ein Ereignis über den Adapter `K` ausgegeben.

Der Baustein führt also eine einfache **Zusammenführung** (Multiplexing) der beiden Eingangsereignisse auf einen einzigen Ausgang durch. Es findet keine Auswahl oder Priorisierung statt – beide Ereignisse werden parallel und in der Reihenfolge ihres Auftretens weitergeleitet. Dies entspricht funktional einer **ODER-Verknüpfung** (OR) auf Ereignisebene.

Da keine Daten-Eingänge oder Auswahlparameter existieren, ist das Verhalten deterministisch und benötigt keine interne Zustandslogik.

## Technische Besonderheiten

- **Adapterbasierte Ausgabe:** Der klassische Daten-Eingang `K` (zur Auswahl) und der separate Ereignis-Ausgang `EO` von `E_MUX_2` wurden durch einen einzigen unidirektionalen AUI-Adapter ersetzt. Dadurch wird die physikalische Schnittstelle vereinfacht und die Kopplung an andere Bausteine über standardisierte Adapter ermöglicht.
- **Generischer Baustein:** Der FB ist als generisch deklariert (`GenericClassName` = `'GEN_E_MUX'`), was bedeutet, dass er in verschiedenen Kontexten mit unterschiedlichen Adaptertypen wiederverwendet werden kann.
- **Keine Zustandsabhängigkeit:** Es gibt keine internen Zustände oder Variablen, die das Verhalten beeinflussen. Das macht den Baustein besonders robust und einfach zu analysieren.
- **Lizenz und Version:** Der Baustein unterliegt der Eclipse Public License 2.0 (EPL-2.0) und wurde für die Verwendung in der 4diac-IDE (61499-1 Annex A) entwickelt.

## Zustandsübersicht

Der Baustein besitzt **keine** expliziten Zustände, da er kein prozedurales Verhalten aufweist. Die Ereignisweiterleitung erfolgt unmittelbar bei jedem Auftreten eines Eingangsereignisses. Es gibt weder Initialisierungs- noch Wartezustände; der FB ist immer bereit, Ereignisse zu verarbeiten.

## Anwendungsszenarien

- **Zusammenführung von Sensorereignissen:** Zwei Sensoren (z.B. Endlage und Not-Aus) senden Ereignisse an den FB; der AUI-Adapter leitet diese an eine zentrale Steuerung weiter.
- **Adapterbasierte Erweiterung:** In Systemen, die bereits mit AUI-Adaptern arbeiten, kann dieser FB verwendet werden, um zwei unabhängige Ereignisquellen ohne zusätzliche Verschaltung auf einen bestehenden Adapterkanal zu legen.
- **Prototypenbau:** Durch die generische Struktur eignet sich der Baustein für schnelle Tests und Simulationen, bei denen Ereignisse ohne aufwendige Multiplexer-Logik gebündelt werden sollen.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Ereignis-Ausgang | Daten-Eingang | Adapter-Ausgang | Auswahlsteuerung |
|----------|------------------|---------------|-----------------|------------------|
| **E_MUX_2**          | Ja (EO) | Ja (K) | Nein | Ja, über K (0/1) |
| **AUI_MUX_2**        | Nein   | Nein  | Ja (K) | Nein (reine OR-Verbindung) |
| **E_OR** (falls vorhanden) | Ja | Nein | Optional | Nein (ODER-Verknüpfung) |

Der wesentliche Unterschied zum klassischen `E_MUX_2` besteht darin, dass der `AUI_MUX_2` keine Möglichkeit bietet, zwischen den Eingängen umzuschalten. Er leitet jedes ankommende Ereignis ungeachtet einer Auswahl weiter. Wenn eine gezielte Auswahl (z.B. über einen Dateneingang oder ein Steuersignal) erforderlich ist, muss der `E_MUX_2` oder eine erweiterte Variante mit Adapter verwendet werden.

## Fazit

Der **AUI_MUX_2** ist ein schlanker, ereignisgesteuerter Baustein, der zwei Ereignisquellen über eine einzige AUI-Adapter-Schnittstelle zusammenführt. Er besticht durch seine einfache Struktur, die vollständige Kompatibilität mit adapterbasierten Architekturen und das Fehlen interner Zustände. Seine Funktionalität beschränkt sich auf die reine Ereignisweitergabe (OR-Verknüpfung) – eine echte Multiplexer-Funktion mit Auswahlsteuerung wird durch diese Variante nicht angeboten. Daher ist er ideal für Anwendungen, bei denen mehrere Ereignisse ohne Priorisierung gebündelt und über einen gemeinsamen Adapter übertragen werden sollen. Für komplexe Multiplexer-Anforderungen mit Auswahl logik sollte auf den Standard-`E_MUX_2` zurückgegriffen werden.

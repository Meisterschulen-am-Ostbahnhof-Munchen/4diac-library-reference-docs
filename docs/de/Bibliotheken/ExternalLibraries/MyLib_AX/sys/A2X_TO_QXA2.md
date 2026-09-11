# A2X_TO_QXA2


![A2X_TO_QXA2_network](./A2X_TO_QXA2_network.svg)

![A2X_TO_QXA2](./A2X_TO_QXA2.svg)

* * * * * * * * * *
## Einleitung

Der Funktionsbaustein **A2X_TO_QXA2** ist eine Subapplikation (Composite) zur Entbündelung eines unidirektionalen A2X-Signals (UP/DOWN) und zur direkten Ansteuerung zweier physischer logiBUS-Digitalausgänge. Die Aufteilung des gebündelten Signals in zwei einzelne AX-Signale erfolgt unmittelbar vor den beiden logiBUS_QXA-Ausgangsbausteinen. Dadurch wird die Entbündelung in die Composite-Struktur verlagert, nicht in die Resource des Geräts.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

| Name        | Typ                         | Initialwert | Kommentar                          |
|-------------|-----------------------------|-------------|-----------------------------------|
| `Output_UP`   | `logiBUS::io::DQ::logiBUS_DO_S` | `Invalid`   | Physischer Ausgang für forward/up   |
| `Output_DOWN` | `logiBUS::io::DQ::logiBUS_DO_S` | `Invalid`   | Physischer Ausgang für backward/down |

### **Daten-Ausgänge**

Keine.

### **Adapter**

| Name | Typ                                   | Richtung | Kommentar                        |
|------|---------------------------------------|----------|---------------------------------|
| `IN` | `adapter::types::unidirectional::A2X` | Socket   | Gebündeltes UP/DOWN Eingangssignal |

## Funktionsweise

Die Subapplikation nimmt über den Socket `IN` ein gebündeltes unidirektionales A2X-Signal entgegen. Dieses Signal enthält zwei logisch getrennte Informationen: UP (Vorwärts) und DOWN (Rückwärts). 

Im Inneren wird das A2X-Signal zunächst dem Funktionsbaustein `UNBUNDLE` (Typ `adapter::conversion::unidirectional::A2X_2X_TO_2AX`) zugeführt. Dieser Baustein splittet das gebündelte Signal in zwei einzelne AX-Signale, die an den Ausgängen `UP` und `DOWN` bereitstehen.

Anschließend werden diese beiden Signale über Adapterverbindungen an die beiden logiBUS_QXA-Ausgangsbausteine `DigitalOutput_UP` und `DigitalOutput_DOWN` übergeben. Die QXA-Bausteine wandeln die digitalen Signale in physische Ausgangswerte um. Die Sollwerte für diese Ausgänge werden über die Daten-Eingänge `Output_UP` und `Output_DOWN` von außen vorgegeben und mit den jeweiligen `Output`-Eingängen der QXA-Bausteine verbunden. Damit steuern die externen Vorgaben die tatsächlichen Ausgangszustände der logiBUS-Digitalausgänge.

Die Verbindungsstruktur ist rein datenflussorientiert – es existiert keine Ereignissteuerung. Die gesamte Logik ist in der Composite-Ebene gekapselt und benötigt keine zusätzlichen Ressourcen-Ressourcen.

## Technische Besonderheiten

- **Composite-Stil:** Die Entbündelung und die physische Ausgangsansteuerung sind als Subapplikation realisiert und können somit in verschiedenen Ressourcen oder Geräten wiederverwendet werden.
- **Verwendung von logiBUS_QXA:** Die Ausgangsbausteine sind vom Typ `logiBUS::io::DQ::logiBUS_QXA` und besitzen den Parameter `QI = TRUE`, wodurch die Ausgänge dauerhaft aktiviert sind.
- **Keine Ereignisverarbeitung:** Der Baustein arbeitet rein signal- bzw. datengetrieben, was die Einbindung in zyklische oder ereignisgesteuerte Umgebungen vereinfacht.
- **Daten-Eingänge als Sollwerte:** `Output_UP` und `Output_DOWN` sind als Strukturvariablen vom Typ `logiBUS_DO_S` definiert und standardmäßig auf `Invalid` gesetzt, was eine explizite Initialisierung durch die Anwendung erfordert.

## Zustandsübersicht

Da es sich um eine rein strukturelle Subapplikation ohne eigene Zustandslogik handelt, existieren keine internen Zustände oder Zustandsübergänge. Die Funktion ergibt sich ausschließlich aus der Verdrahtung der enthaltenen Bausteine.

## Anwendungsszenarien

- **Maschinensteuerung:** Ansteuerung zweier digitaler Ausgänge für Vorwärts-/Rückwärtsbewegungen (z.B. Förderbänder, Antriebe) über eine einzige A2X-Datenverbindung.
- **Logistik- und Materialfluss:** Integration in logiBUS-basierte Steuerungssysteme, bei denen gebündelte Signale auf physische Ausgänge verteilt werden müssen.
- **Modulare Composite-Verwendung:** Wiederverwendbare Einheit für Geräte, die eine direkte Aufteilung von A2X-Signalen auf zwei diskrete Ausgänge benötigen, ohne die Resource selbst mit Entbündelungslogik belasten zu müssen.

## Vergleich mit ähnlichen Bausteinen

- **A2X_2X_TO_2AX (direkt):** Dieser Baustein liefert nur die entbündelten Signale, ohne physische Ansteuerung. `A2X_TO_QXA2` erweitert diese Funktionalität um die Integration der logiBUS-QXA-Ausgangsbausteine.
- **Manuelle Entbündelung auf Resource-Ebene:** Alternativ könnte die Entbündelung direkt in der Resource erfolgen; dieser Composite-Baustein kapselt diesen Schritt und reduziert so den Verbindungsaufwand im übergeordneten System.
- **A2X_TO_2X (mit diskreten Ausgängen):** Falls ausschließlich diskrete Signale ohne physische Ausgangstreiber benötigt werden, wäre dieser Baustein eine schlankere Variante – `A2X_TO_QXA2` ist speziell für die direkte logiBUS-Anbindung optimiert.

## Fazit

Der Subapplikationsbaustein **A2X_TO_QXA2** bietet eine kompakte und wiederverwendbare Lösung, um ein gebündeltes unidirektionales A2X-Signal in zwei physische logiBUS-Digitalausgänge zu überführen. Durch die Integration der Entbündelungs- und Ausgangsbausteine in einer Composite-Struktur wird die Komplexität vom Anwender ferngehalten und die Wartung sowie Wiederverwendung vereinfacht. Die reine Datenflussarchitektur ermöglicht eine unkomplizierte Integration in verschiedene Steuerungsumgebungen.
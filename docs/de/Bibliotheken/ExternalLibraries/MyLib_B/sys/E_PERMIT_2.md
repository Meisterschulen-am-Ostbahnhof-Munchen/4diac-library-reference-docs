# E_PERMIT_2


![E_PERMIT_2_network](./E_PERMIT_2_network.svg)

![E_PERMIT_2](./E_PERMIT_2.svg)

* * * * * * * * * *

## Einleitung

Der Baustein **E_PERMIT_2** ist eine Subapplikation (SubApp), die zwei unabhängige Event-Freigabe-Kanäle realisiert. Er basiert auf zwei Instanzen des Standard-Funktionsblocks `E_PERMIT` aus der IEC 61499-1-Bibliothek. Die SubApp bietet eine kompakte und wiederverwendbare Lösung zur Steuerung von Ereignisdurchgriffen über ein gemeinsames Freigabesignal.

Die Bezeichnung „2-Kanal Event-Freigabe-Gate“ verdeutlicht die Funktion: Ereignisse, die an den Eingängen `EI1` und `EI2` anliegen, werden nur dann an die Ausgänge `EO1` bzw. `EO2` weitergegeben, wenn das Eingangssignal `PERMIT` den Wert `TRUE` besitzt. Andernfalls werden die Ereignisse blockiert.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Datentyp | Kommentar |
|------|----------|-----------|
| EI1  | EVENT    | Ereigniseingang für Kanal 1 |
| EI2  | EVENT    | Ereigniseingang für Kanal 2 |

### **Ereignis-Ausgänge**

| Name | Datentyp | Kommentar |
|------|----------|-----------|
| EO1  | EVENT    | Ereignisausgang für Kanal 1 |
| EO2  | EVENT    | Ereignisausgang für Kanal 2 |

### **Daten-Eingänge**

| Name   | Datentyp | Kommentar                |
|--------|----------|--------------------------|
| PERMIT | BOOL     | Freigabebedingung (gemeinsam für beide Kanäle) |

### **Daten-Ausgänge**

Keine Daten-Ausgänge vorhanden.

### **Adapter**

Keine Adapter vorhanden.

## Funktionsweise

Die SubApp enthält intern zwei `E_PERMIT`-Funktionsblöcke (benannt `E_PERMIT_1` und `E_PERMIT_2`). Das Eingangssignal `PERMIT` wird parallel an beide internen Bausteine weitergeleitet. Ereignisse, die an `EI1` bzw. `EI2` ankommen, werden an den jeweiligen `EI`-Eingang der internen Instanz geführt. Die `EO`-Ausgänge der internen Bausteine sind mit den SubApp-Ausgängen `EO1` und `EO2` verbunden.

Die Funktion eines einzelnen `E_PERMIT`-Bausteins ist wie folgt definiert:

- Ist `PERMIT = TRUE`, werden eingehende Ereignisse (an `EI`) unverändert an `EO` weitergegeben.
- Ist `PERMIT = FALSE`, werden eingehende Ereignisse ignoriert und es wird kein Ereignis am Ausgang erzeugt.

Da beide Kanäle dasselbe Freigabesignal verwenden, verhalten sie sich synchron: Entweder beide Kanäle sind durchlässig oder beide sind gesperrt. Dies ist besonders für Anwendungen sinnvoll, bei denen zwei parallele Ereignispfade gleichzeitig freigegeben oder blockiert werden müssen.

## Technische Besonderheiten

- **SubApp-Implementierung:** Die Funktionalität ist als SubApp (verschachtelte Applikation) realisiert, wodurch sie in größere Systeme eingebettet werden kann und die interne Struktur gekapselt bleibt.
- **Wiederverwendbarkeit:** Der Baustein kann mehrfach in einem Projekt verwendet werden, ohne dass die interne Logik neu modelliert werden muss.
- **Einheitliches Freigabesignal:** Ein einziger boolescher Eingang steuert beide Kanäle, was die Verdrahtung vereinfacht und eine konsistente Steuerung gewährleistet.
- **Keine Datenausgänge:** Der Baustein gibt keine Daten zurück; er dient ausschließlich der Steuerung von Ereignisflüssen.
- **Erweiterbarkeit:** Durch die modulare Struktur lässt sich die SubApp leicht um weitere Kanäle erweitern, indem zusätzliche `E_PERMIT`-Instanzen und entsprechende Verbindungen ergänzt werden.

## Zustandsübersicht

Da es sich um eine SubApp handelt, besitzt der Baustein selbst keine eigenen Zustände. Die Zustandslogik wird vollständig durch die internen `E_PERMIT`-Funktionsblöcke bestimmt. Diese besitzen typischerweise die folgenden Zustände:

- **IDLE:** Wartet auf ein Ereignis am Eingang `EI`.
- **PERMIT_ACTIVE:** Zustand, in dem das Eingangssignal `PERMIT` geprüft wird. (Die genaue Zustandsmaschinen-Definition des `E_PERMIT`-Bausteins liegt außerhalb dieser Dokumentation, wird aber gemäß IEC 61499-Standard umgesetzt.)

Für die SubApp bedeutet dies, dass sie sich in einem „durchlässigen“ oder „blockierenden“ Modus befindet, abhängig vom Wert von `PERMIT`. Der Zustand wechselt nur durch Änderungen des Freigabesignals; Ereignisse werden entweder sofort weitergeleitet oder verworfen.

## Anwendungsszenarien

- **Sicherheitsfreigaben in der Automatisierungstechnik:** Ein übergeordnetes Steuerungssystem setzt `PERMIT` auf `TRUE`, um zwei Prozesspfade (z. B. zwei Ventile oder Antriebe) gleichzeitig zu aktivieren.
- **Test- und Diagnoseschnittstellen:** Freigabe von Ereignissen für zwei unabhängige Überwachungskanäle in einem Diagnosesystem.
- **Redundante Systeme:** Wenn zwei getrennte Kommunikationskanäle nur bei aktiver Freigabe Daten (Ereignisse) senden sollen, kann `E_PERMIT_2` als gemeinsames Gate eingesetzt werden.
- **Bedingte Ereignisweiterleitung in modularen Anlagen:** Ein zentrales Freigabesignal kontrolliert den Durchgriff mehrerer Ereignisströme.

## Vergleich mit ähnlichen Bausteinen

- **E_PERMIT (einfach):** Der einzelne `E_PERMIT`-Baustein steuert nur eine Ereignisverbindung. `E_PERMIT_2` bündelt zwei solche Pfade und reduziert dadurch den Verdrahtungsaufwand auf der übergeordneten Ebene.
- **E_SR (Set/Reset) oder andere Logikbausteine:** Diese arbeiten mit booleschen Signalen, nicht direkt mit Ereignissen. `E_PERMIT_2` ist speziell für die Durchschaltung von Ereignissen ausgelegt und verwendet `PERMIT` als Tor.
- **E_RS, E_CTU etc.:** Nicht vergleichbar, da sie unterschiedliche Funktionalitäten (Set, Reset, Zählen) implementieren.

Gegenüber mehreren separaten `E_PERMIT`-Instanzen bietet `E_PERMIT_2` den Vorteil, dass das Freigabesignal nur einmal angeschlossen werden muss und eine gemeinsame Steuerung semantisch klar abgebildet wird.

## Fazit

`E_PERMIT_2` ist ein nützlicher Baustein für Anwendungen, bei denen zwei Ereigniskanäle durch ein gemeinsames Freigabesignal kontrolliert werden müssen. Die SubApp-Implementierung kapselt die Logik und vereinfacht die Wiederverwendung. Mit nur einem booleschen Eingang und zwei Ereignis-Ein-/Ausgängen ist die Bedienung intuitiv und die Funktionsweise transparent. Dank der modularen Gestaltung lässt sich der Baustein in komplexe Automatisierungssysteme integrieren und bietet eine robuste Lösung für ereignisbasierte Steuerungen.

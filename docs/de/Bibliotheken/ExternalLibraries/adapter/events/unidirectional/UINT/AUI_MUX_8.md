# AUI_MUX_8

![AUI_MUX_8](./AUI_MUX_8.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AUI_MUX_8** ist ein Ereignis-Multiplexer, der acht Eingangsereignisse (EI1 bis EI8) zu einem einzigen Ausgangsereignis bündelt. Das ausgewählte Ereignis wird nicht über einen klassischen Event-Ausgang (EO) ausgegeben, sondern über einen **Adapter** vom Typ `adapter::types::unidirectional::AUI` bereitgestellt. Dies ermöglicht eine flexible und lose Kopplung zwischen dem Multiplexer und nachgeschalteten Funktionsbausteinen oder Subapplikationen.

Der Baustein ist als generischer Funktionsblock implementiert und basiert auf der Kernklasse `GEN_E_MUX`, die in der 4diac-IDE für verschiedene Multiplexer-Varianten verwendet wird.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ    | Kommentar                     |
|----------|--------|-------------------------------|
| `EI1`    | Event  | Ereignis für K = 0            |
| `EI2`    | Event  | Ereignis für K = 1            |
| `EI3`    | Event  | Ereignis für K = 2            |
| `EI4`    | Event  | Ereignis für K = 3            |
| `EI5`    | Event  | Ereignis für K = 4            |
| `EI6`    | Event  | Ereignis für K = 5            |
| `EI7`    | Event  | Ereignis für K = 6            |
| `EI8`    | Event  | Ereignis für K = 7            |

### **Ereignis-Ausgänge**

Keine direkten Event-Ausgänge vorhanden. Die Ausgabe erfolgt über den Adapter `K`.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

| Adapter | Typ                               | Kommentar     |
|---------|-----------------------------------|---------------|
| `K`     | `adapter::types::unidirectional::AUI` | Ereignis-Index (Auswahl des Eingangsereignisses) |

Der Adapter `K` übernimmt eine Doppelfunktion:
- Er empfängt den Auswahlindex (0–7) vom verbundenen Kommunikationspartner.
- Über ihn wird das ausgewählte Eingangsereignis als Adapter-Ereignis an den Partner weitergeleitet.

## Funktionsweise

Der **AUI_MUX_8** wartet auf eines der acht Ereignis-Eingänge `EI1`–`EI8`. Gleichzeitig muss über den Adapter `K` ein gültiger Index (0–7) bereitgestellt werden. Sobald ein Ereignis an einem der Eingänge auftritt, prüft der Funktionsblock den aktuellen Indexwert und leitet das entsprechende Ereignis über den Adapter `K` an den angeschlossenen Baustein weiter.

Die Zuordnung ist:
- `EI1` → Index 0
- `EI2` → Index 1
- ...
- `EI8` → Index 7

Der Baustein arbeitet **ereignisgesteuert**: Nur wenn ein Ereignis an einem Eingang anliegt und der Index gültig ist, wird das Ereignis zum Ausgang transportiert. Ist der Index ungültig (z.B. außerhalb 0–7) oder fehlt ein Ereignis, wird nichts ausgelöst.

## Technische Besonderheiten

- **Adapter statt Ausgang**: Statt eines klassischen Event-Ausgangs wird ein unidirektionaler Adapter (`AUI`) verwendet. Dadurch kann der Multiplexer direkt mit anderen Bausteinen über Adapter-Schnittstellen verbunden werden, ohne zusätzliche Leitungen zu verlegen.
- **Generische Basis**: Der FB nutzt die generische Klasse `GEN_E_MUX` und ist über das Attribut `eclipse4diac::core::GenericClassName` spezifiziert.
- **Paketzuordnung**: Die Compiler-Info verweist auf das Paket `adapter::events::unidirectional`, das für die Adapter-Datentypen und Schnittstellen verantwortlich ist.
- **Versionierung**: Der Baustein wurde mehrfach überarbeitet; aktuelle Version 3.0 (2025-04-14) sowie frühere Versionen von fortiss GmbH und HR Agrartechnik GmbH sind dokumentiert.

## Zustandsübersicht

Der Funktionsblock besitzt keine expliziten Zustände. Sein Verhalten ist rein ereignisbasiert:

- **Bereitschaft**: Kein Eingangsereignis aktiv, keine Ausgabe.
- **Selektion und Weiterleitung**: Eines der Eingangsereignisse trifft ein und wird gemäß Index über den Adapter ausgegeben.

Eine Zustandsautomaten-Darstellung entfällt daher.

## Anwendungsszenarien

- **Ereignis-Routing in verteilten Systemen**: Auswahl verschiedener Ereignisquellen (z.B. Sensoren, Alarme) und Weiterleitung an eine zentrale Verarbeitungslogik.
- **Adapterbasierte Kommunikation**: Einbindung in Steuerungssysteme, bei denen Adapter als standardisierte Schnittstellen zwischen Hardware und Software verwendet werden.
- **Modularisierung**: Einsatz in Subapplikationen, um mehrere asynchrone Ereignisse zu einem kontrollierten Datenstrom zu bündeln.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zum klassischen **E_MUX8**, der einen dedizierten Event-Ausgang `EO` und einen Daten-Eingang `K` besitzt, verwendet **AUI_MUX_8** einen Adapter für die Auswahl und Ausgabe. Das bietet:

- **Flexible Topologien** durch Adapterverbindungen.
- **Reduktion von Verdrahtungsaufwand** (kein separater Datenkanal für den Index notwendig, da der Index direkt über den Adapter übertragen wird).
- **Einschränkung**: Die Auswahl muss über den Adapter erfolgen, was bei sehr einfachen Anwendungen ohne Adapter-Infrastruktur weniger geeignet ist.

## Fazit

Der Funktionsblock **AUI_MUX_8** ist eine moderne, adapterbasierte Variante eines Ereignis-Multiplexers. Er eignet sich besonders für systeme, in denen Kommunikationsstrukturen über Adapter standardisiert werden und eine flexible Anbindung an unterschiedliche Teilnehmer gefordert ist. Durch die Verwendung der generischen Basisklasse ist eine einfache Wiederverwendung und Anpassung in der 4diac-IDE gewährleistet.
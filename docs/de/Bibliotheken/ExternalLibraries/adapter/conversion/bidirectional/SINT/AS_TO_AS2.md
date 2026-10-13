# AS_TO_AS2

![AS_TO_AS2_network](./AS_TO_AS2_network.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `AS_TO_AS2` hängt einen bidirektionalen [`AS2`](../../../types/bidirectional/SINT/AS2.md)-Abzweig ("Tap") in eine unidirektionale [`AS`](../../../types/unidirectional/SINT/AS.md)-Kette (SINT) ein. Er nutzt dafür **drei** Adapter: Ein eingehendes unidirektionales AS-Signal wird auf die bidirektionale AS2-Schnittstelle veröffentlicht, und was AS2 auf seiner Anfrage-Seite zurückliefert, wird als eigenständiges unidirektionales AS-Signal wieder ausgegeben.

![AS_TO_AS2](AS_TO_AS2.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Eingänge.

### **Ereignis-Ausgänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Ausgänge.

### **Daten-Eingänge**

Der Funktionsblock verfügt über keine direkten Daten-Eingänge.

### **Daten-Ausgänge**

Der Funktionsblock verfügt über keine direkten Daten-Ausgänge.

### **Adapter**

- **AS_IN** (Socket): unidirektionale SINT-Quelle, Typ `adapter::types::unidirectional::AS`
- **AS2_BIDIR** (Plug): bidirektionaler Abzweig, dessen ausgehende Seite von `AS_IN` gespeist wird und dessen eingehende Seite an `AS_OUT` weitergereicht wird, Typ `adapter::types::bidirectional::AS2`
- **AS_OUT** (Plug): spiegelt die eingehende (Anfrage-)Seite von `AS2_BIDIR` als unidirektionales Signal, Typ `adapter::types::unidirectional::AS`

## Funktionsweise

Der Baustein arbeitet in zwei Richtungen gleichzeitig: Was am Socket `AS_IN` eintrifft (`E1`/`D1`), wird direkt auf die ausgehende Seite von `AS2_BIDIR` (`EO1`/`DO1`) veröffentlicht. Umgekehrt wird alles, was `AS2_BIDIR` auf seiner Anfrage-Seite empfängt (`EI1`/`DI1`), unverändert als eigenständiges unidirektionales Signal über den Plug `AS_OUT` ausgegeben. `AS2_BIDIR` fungiert damit als Abzweigpunkt, der die unidirektionale Kette nicht unterbricht, sondern um eine bidirektionale Beobachtungs-/Einspeisemöglichkeit erweitert.

## Technische Besonderheiten

- Drei statt zwei Adapter: Die eingehende und die vom Tap zurückkommende Signalrichtung werden bewusst getrennt gehalten
- Reine Verdrahtung ohne Logik oder Zustand, jede Zielvariable hat genau einen Schreiber
- `AS_OUT` gibt nur wieder, was über `AS2_BIDIR` von einem angeschlossenen bidirektionalen Teilnehmer zurückkommt; ohne einen solchen Teilnehmer bleibt `AS_OUT` inaktiv

## Zustandsübersicht

Der Baustein ist zustandslos:

- AS_IN.E1 → AS2_BIDIR.EO1, AS_IN.D1 → AS2_BIDIR.DO1
- AS2_BIDIR.EI1 → AS_OUT.E1, AS2_BIDIR.DI1 → AS_OUT.D1

## Anwendungsszenarien

- Nachträgliches Einhängen eines bidirektionalen Diagnose- oder Steuerpunkts in eine bestehende unidirektionale AS-Signalkette, ohne diese zu unterbrechen
- Brücke zwischen einer unidirektionalen AS-Zone und einer bidirektionalen AS2-Zone

## ⚖️ Vergleich mit ähnlichen Bausteinen

Das Gegenstück in umgekehrter Richtung ist der Baustein `AS2_TO_AS` (dort steht `AS2_BIDIR` als Socket auf der linken Seite). Die Bausteine dieser Konvertierungsfamilie existieren analog für die Adapter `AR`/`AR2`, `AB`/`AB2` und `AE`/`AE2`.

## Fazit

`AS_TO_AS2` ist kein einfacher Konverter, sondern ein Abzweigpunkt: Er macht ein unidirektionales AS-Signal an einem bidirektionalen AS2-Punkt sichtbar, ohne die ursprüngliche Signalkette zu unterbrechen, und gibt das dort empfangene Anfrage-Signal als eigenständiges AS-Signal weiter.

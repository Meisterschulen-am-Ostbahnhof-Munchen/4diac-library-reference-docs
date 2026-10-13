# AUDI_TO_AUDI2

![AUDI_TO_AUDI2_network](./AUDI_TO_AUDI2_network.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `AUDI_TO_AUDI2` hängt einen bidirektionalen [`AUDI2`](../../../types/bidirectional/UDINT/AUDI2.md)-Abzweig ("Tap") in eine unidirektionale [`AUDI`](../../../types/unidirectional/UDINT/AUDI.md)-Kette (UDINT) ein. Er nutzt dafür **drei** Adapter: Ein eingehendes unidirektionales AUDI-Signal wird auf die bidirektionale AUDI2-Schnittstelle veröffentlicht, und was AUDI2 auf seiner Anfrage-Seite zurückliefert, wird als eigenständiges unidirektionales AUDI-Signal wieder ausgegeben.

![AUDI_TO_AUDI2](AUDI_TO_AUDI2.svg)

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

- **AUDI_IN** (Socket): unidirektionale UDINT-Quelle, Typ `adapter::types::unidirectional::AUDI`
- **AUDI2_BIDIR** (Plug): bidirektionaler Abzweig, dessen ausgehende Seite von `AUDI_IN` gespeist wird und dessen eingehende Seite an `AUDI_OUT` weitergereicht wird, Typ `adapter::types::bidirectional::AUDI2`
- **AUDI_OUT** (Plug): spiegelt die eingehende (Anfrage-)Seite von `AUDI2_BIDIR` als unidirektionales Signal, Typ `adapter::types::unidirectional::AUDI`

## Funktionsweise

Der Baustein arbeitet in zwei Richtungen gleichzeitig: Was am Socket `AUDI_IN` eintrifft (`E1`/`D1`), wird direkt auf die ausgehende Seite von `AUDI2_BIDIR` (`EO1`/`DO1`) veröffentlicht. Umgekehrt wird alles, was `AUDI2_BIDIR` auf seiner Anfrage-Seite empfängt (`EI1`/`DI1`), unverändert als eigenständiges unidirektionales Signal über den Plug `AUDI_OUT` ausgegeben. `AUDI2_BIDIR` fungiert damit als Abzweigpunkt, der die unidirektionale Kette nicht unterbricht, sondern um eine bidirektionale Beobachtungs-/Einspeisemöglichkeit erweitert.

## Technische Besonderheiten

- Drei statt zwei Adapter: Die eingehende und die vom Tap zurückkommende Signalrichtung werden bewusst getrennt gehalten
- Reine Verdrahtung ohne Logik oder Zustand, jede Zielvariable hat genau einen Schreiber
- `AUDI_OUT` gibt nur wieder, was über `AUDI2_BIDIR` von einem angeschlossenen bidirektionalen Teilnehmer zurückkommt; ohne einen solchen Teilnehmer bleibt `AUDI_OUT` inaktiv

## Zustandsübersicht

Der Baustein ist zustandslos:

- AUDI_IN.E1 → AUDI2_BIDIR.EO1, AUDI_IN.D1 → AUDI2_BIDIR.DO1
- AUDI2_BIDIR.EI1 → AUDI_OUT.E1, AUDI2_BIDIR.DI1 → AUDI_OUT.D1

## Anwendungsszenarien

- Nachträgliches Einhängen eines bidirektionalen Diagnose- oder Steuerpunkts in eine bestehende unidirektionale AUDI-Signalkette, ohne diese zu unterbrechen
- Brücke zwischen einer unidirektionalen AUDI-Zone und einer bidirektionalen AUDI2-Zone

## ⚖️ Vergleich mit ähnlichen Bausteinen

Das Gegenstück in umgekehrter Richtung ist der Baustein `AUDI2_TO_AUDI` (dort steht `AUDI2_BIDIR` als Socket auf der linken Seite). Die Bausteine dieser Konvertierungsfamilie existieren analog für die Adapter `AR`/`AR2`, `AB`/`AB2` und `AE`/`AE2`.

## Fazit

`AUDI_TO_AUDI2` ist kein einfacher Konverter, sondern ein Abzweigpunkt: Er macht ein unidirektionales AUDI-Signal an einem bidirektionalen AUDI2-Punkt sichtbar, ohne die ursprüngliche Signalkette zu unterbrechen, und gibt das dort empfangene Anfrage-Signal als eigenständiges AUDI-Signal weiter.

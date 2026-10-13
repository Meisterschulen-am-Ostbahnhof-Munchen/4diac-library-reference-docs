# AS2_TO_AS

![AS2_TO_AS_network](./AS2_TO_AS_network.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock AS2_TO_AS ist das Spiegelbild von [AS_TO_AS2](AS_TO_AS2.md): Hier steht der bidirektionale [AS2](../../../types/bidirectional/SINT/AS2.md)-Abzweig als Socket auf der linken (Eingangs-)Seite, ergänzt um einen zweiten unidirektionalen [AS](../../../types/unidirectional/SINT/AS.md)-Socket und einen unidirektionalen AS-Plug.

![AS2_TO_AS](AS2_TO_AS.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Eingänge – die Kommunikation läuft ausschließlich über die Adapter.

### **Ereignis-Ausgänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Ausgänge.

### **Daten-Eingänge**

Der Funktionsblock verfügt über keine direkten Daten-Eingänge.

### **Daten-Ausgänge**

Der Funktionsblock verfügt über keine direkten Daten-Ausgänge.

### **Adapter**

- **AS2_BIDIR** (Socket): bidirektionaler Abzweig, dessen eingehende Seite an `AS_OUT` weitergereicht wird und dessen ausgehende Seite von `AS_IN` gespeist wird, Typ `adapter::types::bidirectional::AS2`
- **AS_IN** (Socket): unidirektionale SINT-Quelle, Typ `adapter::types::unidirectional::AS`
- **AS_OUT** (Plug): spiegelt die eingehende (Indikations-)Seite von `AS2_BIDIR` als unidirektionales Signal, Typ `adapter::types::unidirectional::AS`

## Funktionsweise

Der Baustein arbeitet in zwei Richtungen gleichzeitig: Was der Socket `AS2_BIDIR` auf seiner Indikations-Seite empfängt (`EO1`/`DO1` – also was ein an AS2_BIDIR angeschlossener bidirektionaler Teilnehmer meldet), wird unverändert als eigenständiges unidirektionales Signal über den Plug `AS_OUT` ausgegeben. Umgekehrt wird alles, was am Socket `AS_IN` eintrifft (`E1`/`D1`), auf die Anfrage-Seite von `AS2_BIDIR` (`EI1`/`DI1`) veröffentlicht – AS2_BIDIR sendet also dasselbe Signal an den angeschlossenen bidirektionalen Teilnehmer weiter.

## Technische Besonderheiten

- Drei statt zwei Adapter: Die vom Tap empfangene und die separat eingespeiste Signalrichtung werden bewusst getrennt gehalten, statt sie zu vermischen
- Reine Verdrahtung ohne Logik oder Zustand, jede Zielvariable hat genau einen Schreiber
- `AS_OUT` gibt nur wieder, was über `AS2_BIDIR` von einem angeschlossenen bidirektionalen Teilnehmer gemeldet wird – ohne einen solchen Teilnehmer bleibt `AS_OUT` inaktiv

## Zustandsübersicht

Der Baustein ist zustandslos:

- AS2_BIDIR.EO1 → AS_OUT.E1, AS2_BIDIR.DO1 → AS_OUT.D1
- AS_IN.E1 → AS2_BIDIR.EI1, AS_IN.D1 → AS2_BIDIR.DI1

## Anwendungsszenarien

- Auslesen des Status eines bidirektionalen AS2-Teilnehmers als einfaches unidirektionales AS-Signal, kombiniert mit dem Einspeisen eines separaten unidirektionalen Kommandosignals
- Systeme, in denen ein bidirektionaler Teilnehmer überwacht werden soll, ohne die restliche Steuerung auf AS2 umzustellen
- Brücke zwischen einer bidirektionalen AS2-Zone und einer unidirektionalen AS-Zone

## ⚖️ Vergleich mit ähnlichen Bausteinen

[AS_TO_AS2](AS_TO_AS2.md) ist das Spiegelbild dieses Bausteins – dort steht `AS2_BIDIR` als Plug auf der rechten Seite.

## Fazit

AS2_TO_AS ist kein einfacher Konverter, sondern ein Abzweigpunkt: Es macht den Zustand eines bidirektionalen AS2-Teilnehmers als eigenständiges unidirektionales AS-Signal sichtbar und speist gleichzeitig ein separates unidirektionales Signal in die bidirektionale Seite ein.

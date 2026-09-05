# AE_TO_AE_AX_SPLIT

![AE_TO_AE_AX_SPLIT](AE_TO_AE_AX_SPLIT.svg)

* * * * * * * * * *

## Einleitung

Der AE_TO_AE_AX_SPLIT ist ein Composite-Funktionsblock, der ein eingehendes, **unidirektionales** AE-Ereignis an seinem Socket `IN` in ein **bidirektionales** AE_AX-Signal am Plug `OUT` umwandelt, und zusätzlich den auf dem Rückkanal von `OUT` gemeldeten Zustand (Event + Bool) über einen dritten, unidirektionalen `AX_OUT`-Plug nach außen spiegelt. Anders als beim reinen Passthrough [`AE_AX_AX_SPLIT`](AE_AX_AX_SPLIT.md) hat `IN` hier keinen eigenen Rückkanal - der Rückkanal kann also nicht an `IN` zurückgemeldet werden und läuft ausschließlich über `AX_OUT` nach außen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

*Keine direkten Ereignis-Eingänge vorhanden – Ereignisse kommen über die Adapter-Sockets/-Plugs*

### **Ereignis-Ausgänge**

*Keine direkten Ereignis-Ausgänge vorhanden*

### **Daten-Eingänge**

*Keine Daten-Eingänge vorhanden*

### **Daten-Ausgänge**

*Keine Daten-Ausgänge vorhanden*

### **Adapter**

- **IN**: Unidirektionaler Adapter-Socket vom Typ `adapter::types::unidirectional::AE` (Eingang, kein Rückkanal)
- **OUT**: Bidirektionaler Adapter-Plug vom Typ `adapter::types::bidirectional::AE_AX` (Ausgang, mit Rückkanal)
- **AX_OUT**: Unidirektionaler Adapter-Plug vom Typ `adapter::types::unidirectional::AX`, spiegelt den Rückkanal (Zustand) von AE_AX nach außen

## Funktionsweise

1. Jedes an `IN.E1` eintreffende Ereignis wird unverändert an `OUT.E1` weitergeleitet.
2. Das Rückkanal-Ereignis `OUT.EI1` (samt zugehörigem Datum `OUT.DI1`), das die nachgeschaltete Gegenstelle über `OUT` sendet, wird ausschließlich an `AX_OUT.E1`/`AX_OUT.D1` weitergegeben - eine Rückmeldung an `IN` ist technisch unmöglich, da `IN` als unidirektionaler AE-Adapter keinen Rückkanal besitzt.
3. `AX_OUT` ist damit der einzige Ort, an dem der von `OUT` gemeldete Zustand sichtbar wird.

## Technische Besonderheiten

- Reine Ereignis-/Datenverbindungen (`FBNetwork`), keine eigene Logik oder Zustandsverwaltung
- Vorwärtsrichtung (Socket → Plug) ist ein einfacher 1:1-Passthrough
- Wandelt einen unidirektionalen Adapter (`IN`) in einen bidirektionalen (`OUT`) um, ohne selbst einen Rückkanal am Eingang zu benötigen
- Der Rückkanal wird nicht verdoppelt (wie bei AE_AX_AX_SPLIT), sondern einzig über `AX_OUT` exponiert

## Zustandsübersicht

Der Funktionsblock besitzt keinen internen Zustand und arbeitet stateless. Jedes eingehende Ereignis wird sofort weitergeleitet bzw. gespiegelt.

## Anwendungsszenarien

- Anschluss eines bestehenden, rein unidirektionalen AE-Signalgebers an eine Kette, die einen bidirektionalen AE_AX-Adapter erwartet
- Sichtbarmachen des Zustands der nachgeschalteten AE_AX-Gegenstelle über `AX_OUT`, obwohl die ursprüngliche Quelle (`IN`) selbst keinen Rückkanal unterstützt

## ⚖️ Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu [`AE_AX_AX_SPLIT`](AE_AX_AX_SPLIT.md), das zwischen zwei bereits bidirektionalen Adaptern vermittelt und den Rückkanal zusätzlich an `IN` zurückspiegelt, wandelt AE_TO_AE_AX_SPLIT einen unidirektionalen Eingang in einen bidirektionalen Ausgang um - der Rückkanal kann daher nur über `AX_OUT` beobachtet werden. Die Set/Reset- bzw. Set/Reset/Toggle-Varianten [`ASR_TO_ASR_AX_SPLIT`](ASR_TO_ASR_AX_SPLIT.md) und [`ASRT_TO_ASRT_AX_SPLIT`](ASRT_TO_ASRT_AX_SPLIT.md) folgen demselben Muster mit zwei bzw. drei Vorwärts-Ereignissen statt einem einzelnen.

## Fazit

Der AE_TO_AE_AX_SPLIT ermöglicht den Anschluss eines rein unidirektionalen AE-Signalgebers an eine bidirektionale AE_AX-Kette und macht deren Rückkanal isoliert über `AX_OUT` verfügbar, ohne dass am ursprünglichen Eingang selbst eine Rückmeldung möglich sein muss.

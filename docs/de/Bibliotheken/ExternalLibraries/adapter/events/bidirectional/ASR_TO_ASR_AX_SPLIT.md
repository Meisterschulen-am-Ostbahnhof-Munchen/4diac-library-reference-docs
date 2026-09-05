# ASR_TO_ASR_AX_SPLIT

![ASR_TO_ASR_AX_SPLIT](ASR_TO_ASR_AX_SPLIT.svg)

* * * * * * * * * *

## Einleitung

Der ASR_TO_ASR_AX_SPLIT ist die Set/Reset-Variante von [`AE_TO_AE_AX_SPLIT`](AE_TO_AE_AX_SPLIT.md): er wandelt ein eingehendes, **unidirektionales** ASR-Signal (Set/Reset, ohne Rückkanal) an seinem Socket `IN` in ein **bidirektionales** ASR_AX-Signal am Plug `OUT` um, und spiegelt den auf dem Rückkanal von `OUT` gemeldeten Zustand zusätzlich über einen dritten, unidirektionalen `AX_OUT`-Plug nach außen.

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

- **IN**: Unidirektionaler Adapter-Socket vom Typ `adapter::types::unidirectional::ASR` (Set/Reset-Eingang, kein Rückkanal)
- **OUT**: Bidirektionaler Adapter-Plug vom Typ `adapter::types::bidirectional::ASR_AX` (Set/Reset-Ausgang, mit Rückkanal)
- **AX_OUT**: Unidirektionaler Adapter-Plug vom Typ `adapter::types::unidirectional::AX`, spiegelt den Rückkanal (Zustand) von ASR_AX nach außen

## Funktionsweise

1. Jedes an `IN.SET` eintreffende Ereignis wird unverändert an `OUT.SET` weitergeleitet, ebenso jedes `IN.RESET` an `OUT.RESET` - beide Ereignispfade laufen unabhängig voneinander durch.
2. Das Rückkanal-Ereignis `OUT.EI1` (samt zugehörigem Datum `OUT.DI1`), das die nachgeschaltete Gegenstelle über `OUT` sendet, wird ausschließlich an `AX_OUT.E1`/`AX_OUT.D1` weitergegeben - eine Rückmeldung an `IN` ist technisch unmöglich, da `IN` als unidirektionaler ASR-Adapter keinen Rückkanal besitzt.
3. `AX_OUT` ist damit der einzige Ort, an dem der von `OUT` gemeldete Zustand sichtbar wird.

## Technische Besonderheiten

- Reine Ereignis-/Datenverbindungen (`FBNetwork`), keine eigene Logik oder Zustandsverwaltung
- Zwei unabhängige Vorwärtspfade (SET, RESET), jeweils ein einfacher 1:1-Passthrough
- Wandelt einen unidirektionalen Adapter (`IN`) in einen bidirektionalen (`OUT`) um, ohne selbst einen Rückkanal am Eingang zu benötigen
- Der Rückkanal wird nicht verdoppelt, sondern einzig über `AX_OUT` exponiert

## Zustandsübersicht

Der Funktionsblock besitzt keinen internen Zustand und arbeitet stateless. Jedes eingehende Ereignis wird sofort weitergeleitet bzw. gespiegelt.

## Anwendungsszenarien

- Anschluss eines bestehenden, rein unidirektionalen ASR-Signalgebers (z. B. ein einfacher Set/Reset-Taster) an eine Kette, die einen bidirektionalen ASR_AX-Adapter erwartet
- Sichtbarmachen des Zustands der nachgeschalteten ASR_AX-Gegenstelle über `AX_OUT`, obwohl die ursprüngliche Quelle (`IN`) selbst keinen Rückkanal unterstützt

## ⚖️ Vergleich mit ähnlichen Bausteinen

Strukturell identisch zu [`AE_TO_AE_AX_SPLIT`](AE_TO_AE_AX_SPLIT.md), nur mit zwei statt einem Vorwärts-Ereignis (SET, RESET). Für die zusätzliche TOGGLE-Variante siehe [`ASRT_TO_ASRT_AX_SPLIT`](ASRT_TO_ASRT_AX_SPLIT.md).

## Fazit

Der ASR_TO_ASR_AX_SPLIT ermöglicht den Anschluss eines rein unidirektionalen ASR-Signalgebers an eine bidirektionale ASR_AX-Kette und macht deren Rückkanal isoliert über `AX_OUT` verfügbar, ohne dass am ursprünglichen Eingang selbst eine Rückmeldung möglich sein muss.

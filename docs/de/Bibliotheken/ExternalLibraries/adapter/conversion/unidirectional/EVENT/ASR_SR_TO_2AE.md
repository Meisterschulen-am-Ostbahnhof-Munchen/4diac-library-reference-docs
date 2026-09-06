# ASR_SR_TO_2AE

![ASR_SR_TO_2AE](ASR_SR_TO_2AE.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **ASR_SR_TO_2AE** ist ein Composite-FB, der einen unidirektionalen ASR-Adapter (Set/Reset-Ereignispaar) in zwei separate AE-Adapter (reines Ereignis, keine Nutzdaten) zerlegt. Er ist die Umkehrung von [ASR_2AE_TO_SR](ASR_2AE_TO_SR.md) und die adapterbasierte Entsprechung von [ASR_SR_TO_2EVENTS](ASR_SR_TO_2EVENTS.md).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine. Die Ereignisübernahme erfolgt ausschließlich über den Adapter-Socket.

### **Ereignis-Ausgänge**

Keine. Die Ereignisweitergabe erfolgt ausschließlich über die Adapter-Plugs.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

| Rolle | Name | Typ | Beschreibung |
| ------- | ------ | ----- | -------------- |
| Socket | `ASR_IN` | `adapter::types::unidirectional::ASR` | Eingehendes Set/Reset-Signal. |
| Plug | `SET_OUT` | `adapter::types::unidirectional::AE` | Set / Einschalten. |
| Plug | `RESET_OUT` | `adapter::types::unidirectional::AE` | Reset / Ausschalten. |

## Funktionsweise

Der Baustein ist ein reines FBNetwork aus zwei Ereignisverbindungen ohne eigene Algorithmen: `ASR_IN.SET` wird direkt auf `SET_OUT.E1` geführt, `ASR_IN.RESET` direkt auf `RESET_OUT.E1`. Ein SET- bzw. RESET-Ereignis am ASR-Socket löst unmittelbar das entsprechende Ereignis am jeweiligen AE-Plug aus.

## Technische Besonderheiten

- **Reine Verdrahtung**: Composite-FB ohne ECC oder Algorithmen, ausschließlich zwei direkte Ereignisverbindungen.
- **Adapterbasiert statt Event-basiert**: Im Gegensatz zu [ASR_SR_TO_2EVENTS](ASR_SR_TO_2EVENTS.md) (klassische Ereignisausgänge) liefert dieser Baustein AE-Adapter-Plugs, wodurch er sich nahtlos in adapterbasierte Netzwerke einfügt.

## Zustandsübersicht

Der Baustein besitzt **keinen Zustandsautomaten**. Jedes `SET`- oder `RESET`-Ereignis an `ASR_IN` wird unmittelbar an `SET_OUT` bzw. `RESET_OUT` weitergereicht.

## Anwendungsszenarien

- **Auffächern eines ASR-Signals** in zwei unabhängig weiterverarbeitbare AE-Ereignisse, z. B. um SET und RESET getrennt an unterschiedliche Folgebausteine zu leiten.
- **Adapterisierung** bestehender ASR-basierter Netzwerke für Weiterverarbeitung mit reinen AE-Adaptern.

## Vergleich mit ähnlichen Bausteinen

- **[ASR_2AE_TO_SR](ASR_2AE_TO_SR.md)**: die Umkehrrichtung – führt zwei AE-Adapter zu einem ASR-Signal zusammen.
- **[ASR_SR_TO_2EVENTS](ASR_SR_TO_2EVENTS.md)**: dieselbe Funktion mit klassischen Ereignisausgängen statt AE-Adapter-Plugs.
- **[ASRT_SRT_TO_3AE](ASRT_SRT_TO_3AE.md)**: die um `TOGGLE` erweiterte Variante für ASRT.

## Fazit

`ASR_SR_TO_2AE` ist eine einfache, reine Verdrahtungslösung zur Zerlegung eines ASR-Adapters in zwei AE-Adapter und eignet sich zur nahtlosen Integration von ASR-Signalen in AE-basierte Netzwerke.

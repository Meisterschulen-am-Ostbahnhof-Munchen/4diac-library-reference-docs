# ASRT_SRT_TO_3AE

![ASRT_SRT_TO_3AE](ASRT_SRT_TO_3AE.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **ASRT_SRT_TO_3AE** ist ein Composite-FB, der einen unidirektionalen ASRT-Adapter (Set/Reset/Toggle-Ereignistripel) in drei separate AE-Adapter (reines Ereignis, keine Nutzdaten) zerlegt. Er ist die Umkehrung von [ASRT_3AE_TO_SRT](ASRT_3AE_TO_SRT.md) und die adapterbasierte Entsprechung von [ASRT_SRT_TO_3EVENTS](ASRT_SRT_TO_3EVENTS.md).

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
| Socket | `ASRT_IN` | `adapter::types::unidirectional::ASRT` | Eingehendes Set/Reset/Toggle-Signal. |
| Plug | `SET_OUT` | `adapter::types::unidirectional::AE` | Set / Einschalten. |
| Plug | `RESET_OUT` | `adapter::types::unidirectional::AE` | Reset / Ausschalten. |
| Plug | `TOGGLE_OUT` | `adapter::types::unidirectional::AE` | Toggle / Ausgang umkehren. |

## Funktionsweise

Der Baustein ist ein reines FBNetwork aus drei Ereignisverbindungen ohne eigene Algorithmen: `ASRT_IN.SET` wird direkt auf `SET_OUT.E1` geführt, `ASRT_IN.RESET` auf `RESET_OUT.E1` und `ASRT_IN.TOGGLE` auf `TOGGLE_OUT.E1`. Ein SET-, RESET- oder TOGGLE-Ereignis am ASRT-Socket löst unmittelbar das entsprechende Ereignis am jeweiligen AE-Plug aus.

## Technische Besonderheiten

- **Reine Verdrahtung**: Composite-FB ohne ECC oder Algorithmen, ausschließlich drei direkte Ereignisverbindungen.
- **Adapterbasiert statt Event-basiert**: Im Gegensatz zu [ASRT_SRT_TO_3EVENTS](ASRT_SRT_TO_3EVENTS.md) (klassische Ereignisausgänge) liefert dieser Baustein AE-Adapter-Plugs, wodurch er sich nahtlos in adapterbasierte Netzwerke einfügt.

## Zustandsübersicht

Der Baustein besitzt **keinen Zustandsautomaten**. Jedes `SET`-, `RESET`- oder `TOGGLE`-Ereignis an `ASRT_IN` wird unmittelbar an den jeweiligen AE-Plug weitergereicht.

## Anwendungsszenarien

- **Auffächern eines ASRT-Signals** (z. B. von `AX_T_FF_SR`) in drei unabhängig weiterverarbeitbare AE-Ereignisse.
- **Adapterisierung** bestehender ASRT-basierter Netzwerke für Weiterverarbeitung mit reinen AE-Adaptern.

## Vergleich mit ähnlichen Bausteinen

- **[ASRT_3AE_TO_SRT](ASRT_3AE_TO_SRT.md)**: die Umkehrrichtung – führt drei AE-Adapter zu einem ASRT-Signal zusammen.
- **[ASRT_SRT_TO_3EVENTS](ASRT_SRT_TO_3EVENTS.md)**: dieselbe Funktion mit klassischen Ereignisausgängen statt AE-Adapter-Plugs.
- **[ASR_SR_TO_2AE](ASR_SR_TO_2AE.md)**: die um `TOGGLE` reduzierte Variante für ASR.
- **[ASRT_SRT_TO_SR_AE](ASRT_SRT_TO_SR_AE.md)**: dieselbe Zerlegung, jedoch mit `SET`/`RESET` als fertigem ASR-Adapter statt zwei einzelnen AE-Plugs.

## Fazit

`ASRT_SRT_TO_3AE` ist eine einfache, reine Verdrahtungslösung zur Zerlegung eines ASRT-Adapters in drei AE-Adapter und eignet sich zur nahtlosen Integration von ASRT-Signalen in AE-basierte Netzwerke.

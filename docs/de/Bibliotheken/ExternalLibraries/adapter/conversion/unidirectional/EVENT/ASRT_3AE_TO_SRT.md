# ASRT_3AE_TO_SRT

![ASRT_3AE_TO_SRT](ASRT_3AE_TO_SRT.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **ASRT_3AE_TO_SRT** ist ein Composite-FB, der drei separate unidirektionale AE-Adapter (reines Ereignis, keine Nutzdaten) zu einem gemeinsamen ASRT-Adapter (Set/Reset/Toggle-Ereignistripel) zusammenführt. Er ist die um `TOGGLE` erweiterte Variante von [ASR_2AE_TO_SR](ASR_2AE_TO_SR.md) und die adapterbasierte Entsprechung von [ASRT_3EVENTS_TO_SRT](ASRT_3EVENTS_TO_SRT.md).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine. Die Ereignisübernahme erfolgt ausschließlich über die Adapter-Sockets.

### **Ereignis-Ausgänge**

Keine. Die Ereignisweitergabe erfolgt ausschließlich über den Adapter-Plug.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

| Rolle | Name | Typ | Beschreibung |
| ------- | ------ | ----- | -------------- |
| Socket | `SET_IN` | `adapter::types::unidirectional::AE` | Set / Einschalten. |
| Socket | `RESET_IN` | `adapter::types::unidirectional::AE` | Reset / Ausschalten. |
| Socket | `TOGGLE_IN` | `adapter::types::unidirectional::AE` | Toggle / Ausgang umkehren. |
| Plug | `ASRT_OUT` | `adapter::types::unidirectional::ASRT` | Zusammengeführtes Set/Reset/Toggle-Signal. |

## Funktionsweise

Der Baustein ist ein reines FBNetwork aus drei Ereignisverbindungen ohne eigene Algorithmen: `SET_IN.E1` wird direkt auf `ASRT_OUT.SET` geführt, `RESET_IN.E1` auf `ASRT_OUT.RESET` und `TOGGLE_IN.E1` auf `ASRT_OUT.TOGGLE`. Ein Ereignis am jeweiligen AE-Socket löst unmittelbar das entsprechende Ereignis am ASRT-Plug aus.

## Technische Besonderheiten

- **Reine Verdrahtung**: Composite-FB ohne ECC oder Algorithmen, ausschließlich drei direkte Ereignisverbindungen.
- **Adapterbasiert statt Event-basiert**: Im Gegensatz zu [ASRT_3EVENTS_TO_SRT](ASRT_3EVENTS_TO_SRT.md) (klassische Ereigniseingänge) verwendet dieser Baustein AE-Adapter-Sockets, wodurch er sich nahtlos in adapterbasierte Netzwerke einfügt.

## Zustandsübersicht

Der Baustein besitzt **keinen Zustandsautomaten**. Jedes Ereignis an `SET_IN`, `RESET_IN` oder `TOGGLE_IN` wird unmittelbar als gleichartiges Ereignis an `ASRT_OUT` weitergereicht.

## Anwendungsszenarien

- **Zusammenführen dreier AE-Signalquellen** (z. B. aus [ASRT_MERGE_2](../../../events/unidirectional/EVENT/ASRT_MERGE_2.md) oder direkt von Sensoren) zu einem einzigen ASRT-Adapter für nachgeschaltete Set/Reset/Toggle-Logik wie `AX_T_FF_SR`.
- **Adapterisierung** bestehender AE-basierter Netzwerke, um sie an ASRT-Schnittstellen anzubinden.

## Vergleich mit ähnlichen Bausteinen

- **[ASRT_SRT_TO_3AE](ASRT_SRT_TO_3AE.md)**: die Umkehrrichtung – zerlegt ein ASRT-Signal wieder in drei AE-Adapter.
- **[ASRT_3EVENTS_TO_SRT](ASRT_3EVENTS_TO_SRT.md)**: dieselbe Funktion mit klassischen Ereigniseingängen statt AE-Adapter-Sockets.
- **[ASR_2AE_TO_SR](ASR_2AE_TO_SR.md)**: die um `TOGGLE` reduzierte Variante für ASR.
- **[ASRT_SR_AE_TO_SRT](ASRT_SR_AE_TO_SRT.md)**: dieselbe Zusammenführung, jedoch mit `SET`/`RESET` bereits als fertigem ASR-Adapter statt zwei einzelnen AE-Sockets.

## Fazit

`ASRT_3AE_TO_SRT` ist eine einfache, reine Verdrahtungslösung zur Zusammenführung dreier AE-Adapter zu einem ASRT-Adapter und eignet sich zur nahtlosen Integration von AE-basierten Ereignisquellen in ASRT-Schnittstellen.

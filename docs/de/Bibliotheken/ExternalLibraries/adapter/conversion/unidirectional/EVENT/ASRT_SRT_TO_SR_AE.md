# ASRT_SRT_TO_SR_AE

![ASRT_SRT_TO_SR_AE](ASRT_SRT_TO_SR_AE.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **ASRT_SRT_TO_SR_AE** ist ein Composite-FB, der einen unidirektionalen ASRT-Adapter (Set/Reset/Toggle) in einen ASR-Adapter (Set/Reset) und einen separaten AE-Adapter (Toggle, reines Ereignis) zerlegt. Er ist die Umkehrung von [ASRT_SR_AE_TO_SRT](ASRT_SR_AE_TO_SRT.md) und eine gemischte Variante von [ASRT_SRT_TO_3AE](ASRT_SRT_TO_3AE.md).

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
| Plug | `SR_OUT` | `adapter::types::unidirectional::ASR` | Set/Reset-Ausgang. |
| Plug | `TOGGLE_OUT` | `adapter::types::unidirectional::AE` | Toggle / Ausgang umkehren. |

## Funktionsweise

Der Baustein ist ein reines FBNetwork aus drei Ereignisverbindungen ohne eigene Algorithmen: `ASRT_IN.SET` wird direkt auf `SR_OUT.SET` geführt, `ASRT_IN.RESET` auf `SR_OUT.RESET` und `ASRT_IN.TOGGLE` auf `TOGGLE_OUT.E1`. Ein SET-, RESET- oder TOGGLE-Ereignis am ASRT-Socket löst unmittelbar das entsprechende Ereignis an `SR_OUT` bzw. `TOGGLE_OUT` aus.

## Technische Besonderheiten

- **Reine Verdrahtung**: Composite-FB ohne ECC oder Algorithmen, ausschließlich drei direkte Ereignisverbindungen.
- **Gemischte Adapter-Ausgänge**: Liefert SET/RESET bereits als fertigen ASR-Adapter statt zweier einzelner AE-Plugs — sinnvoll, wenn die Folgelogik ohnehin einen ASR-Eingang erwartet (z. B. `ASR_AX_SR`) und nur TOGGLE separat behandelt werden soll.

## Zustandsübersicht

Der Baustein besitzt **keinen Zustandsautomaten**. Jedes `SET`-, `RESET`- oder `TOGGLE`-Ereignis an `ASRT_IN` wird unmittelbar an `SR_OUT` bzw. `TOGGLE_OUT` weitergereicht.

## Anwendungsszenarien

- **Anbindung an bestehende ASR-Schnittstellen**: Ein ASRT-Signal soll an einen Baustein weitergegeben werden, der nur SET/RESET als ASR-Adapter erwartet, während TOGGLE separat (z. B. für Diagnosezwecke) abgegriffen wird.
- **Vereinfachung nachgeschalteter Logik**, die Set/Reset ohnehin gebündelt als ASR-Adapter verarbeitet.

## Vergleich mit ähnlichen Bausteinen

- **[ASRT_SR_AE_TO_SRT](ASRT_SR_AE_TO_SRT.md)**: die Umkehrrichtung – führt ASR (SET/RESET) und AE (TOGGLE) zu einem ASRT-Signal zusammen.
- **[ASRT_SRT_TO_3AE](ASRT_SRT_TO_3AE.md)**: dieselbe Zerlegung, jedoch mit drei einzelnen AE-Plugs statt einem fertigen ASR-Adapter für SET/RESET.
- **[ASR_SR_TO_2AE](ASR_SR_TO_2AE.md)**: die um `TOGGLE` reduzierte Variante für ASR.

## Fazit

`ASRT_SRT_TO_SR_AE` ist eine einfache, reine Verdrahtungslösung, die ein ASRT-Signal in einen ASR-Adapter (SET/RESET) und ein separates Toggle-Ereignis auftrennt.

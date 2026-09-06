# ASRT_SR_AE_TO_SRT

![ASRT_SR_AE_TO_SRT](ASRT_SR_AE_TO_SRT.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **ASRT_SR_AE_TO_SRT** ist ein Composite-FB, der einen unidirektionalen ASR-Adapter (Set/Reset) und einen separaten AE-Adapter (Toggle, reines Ereignis) zu einem gemeinsamen ASRT-Adapter (Set/Reset/Toggle) zusammenführt. Er ist eine gemischte Variante von [ASRT_3AE_TO_SRT](ASRT_3AE_TO_SRT.md): Statt drei einzelner AE-Sockets für SET, RESET und TOGGLE nimmt er SET/RESET bereits als fertigen ASR-Adapter entgegen und ergänzt nur TOGGLE separat.

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
| Socket | `SR_IN` | `adapter::types::unidirectional::ASR` | Eingehendes Set/Reset-Signal. |
| Socket | `TOGGLE_IN` | `adapter::types::unidirectional::AE` | Toggle / Ausgang umkehren. |
| Plug | `ASRT_OUT` | `adapter::types::unidirectional::ASRT` | Zusammengeführtes Set/Reset/Toggle-Signal. |

## Funktionsweise

Der Baustein ist ein reines FBNetwork aus drei Ereignisverbindungen ohne eigene Algorithmen: `SR_IN.SET` wird direkt auf `ASRT_OUT.SET` geführt, `SR_IN.RESET` auf `ASRT_OUT.RESET` und `TOGGLE_IN.E1` auf `ASRT_OUT.TOGGLE`. Ein SET- oder RESET-Ereignis am ASR-Socket bzw. ein Ereignis am AE-Socket löst unmittelbar das entsprechende Ereignis am ASRT-Plug aus.

## Technische Besonderheiten

- **Reine Verdrahtung**: Composite-FB ohne ECC oder Algorithmen, ausschließlich drei direkte Ereignisverbindungen.
- **Gemischte Adapter-Eingänge**: Kombiniert einen bereits fertigen ASR-Adapter (SET/RESET) mit einem separaten AE-Adapter (TOGGLE) — sinnvoll, wenn SET/RESET bereits als ASR-Signal vorliegen (z. B. aus [ASR_2AE_TO_SR](ASR_2AE_TO_SR.md) oder einem bestehenden ASR-Netzwerkzweig) und nur TOGGLE zusätzlich ergänzt werden muss.

## Zustandsübersicht

Der Baustein besitzt **keinen Zustandsautomaten**. Jedes Ereignis an `SR_IN` (SET/RESET) oder `TOGGLE_IN` wird unmittelbar als gleichartiges Ereignis an `ASRT_OUT` weitergereicht.

## Anwendungsszenarien

- **Nachträgliches Ergänzen von TOGGLE**: Ein bestehendes ASR-Netzwerk soll um eine Toggle-Funktion erweitert werden, ohne SET/RESET neu zu verdrahten.
- **Wiederverwendung vorhandener ASR-Signalpfade** in Kombination mit einer separaten Toggle-Quelle, z. B. einem Taster über [ASR_MERGE_2](../../../events/unidirectional/EVENT/ASR_MERGE_2.md).

## Vergleich mit ähnlichen Bausteinen

- **[ASRT_SRT_TO_SR_AE](ASRT_SRT_TO_SR_AE.md)**: die Umkehrrichtung – zerlegt ein ASRT-Signal wieder in ASR (SET/RESET) und AE (TOGGLE).
- **[ASRT_3AE_TO_SRT](ASRT_3AE_TO_SRT.md)**: dieselbe Zusammenführung, jedoch mit drei einzelnen AE-Sockets statt einem fertigen ASR-Adapter für SET/RESET.
- **[ASR_2AE_TO_SR](ASR_2AE_TO_SR.md)**: die um `TOGGLE` reduzierte Variante für ASR.

## Fazit

`ASRT_SR_AE_TO_SRT` ist eine einfache, reine Verdrahtungslösung, die einen bestehenden ASR-Adapter um ein separates Toggle-Ereignis zu einem vollständigen ASRT-Adapter ergänzt.

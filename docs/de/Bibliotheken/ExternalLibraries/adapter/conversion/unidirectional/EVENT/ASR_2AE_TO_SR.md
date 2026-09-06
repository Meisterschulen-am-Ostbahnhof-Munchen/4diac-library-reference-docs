# ASR_2AE_TO_SR

![ASR_2AE_TO_SR](ASR_2AE_TO_SR.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **ASR_2AE_TO_SR** ist ein Composite-FB, der zwei separate unidirektionale AE-Adapter (reines Ereignis, keine Nutzdaten) zu einem gemeinsamen ASR-Adapter (Set/Reset-Ereignispaar) zusammenführt. Er ist die adapterbasierte Entsprechung von [ASR_2EVENTS_TO_SR](ASR_2EVENTS_TO_SR.md), verwendet jedoch AE-Adapter-Sockets statt klassischer Ereigniseingänge.

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
| Plug | `ASR_OUT` | `adapter::types::unidirectional::ASR` | Zusammengeführtes Set/Reset-Signal. |

## Funktionsweise

Der Baustein ist ein reines FBNetwork aus zwei Ereignisverbindungen ohne eigene Algorithmen: `SET_IN.E1` wird direkt auf `ASR_OUT.SET` geführt, `RESET_IN.E1` direkt auf `ASR_OUT.RESET`. Ein Ereignis am jeweiligen AE-Socket löst unmittelbar das entsprechende SET- bzw. RESET-Ereignis am ASR-Plug aus.

## Technische Besonderheiten

- **Reine Verdrahtung**: Composite-FB ohne ECC oder Algorithmen, ausschließlich zwei direkte Ereignisverbindungen.
- **Adapterbasiert statt Event-basiert**: Im Gegensatz zu [ASR_2EVENTS_TO_SR](ASR_2EVENTS_TO_SR.md) (klassische Ereigniseingänge `SET`/`RESET`) verwendet dieser Baustein AE-Adapter-Sockets, wodurch er sich nahtlos in adapterbasierte Netzwerke einfügt.

## Zustandsübersicht

Der Baustein besitzt **keinen Zustandsautomaten**. Jedes Ereignis an `SET_IN` oder `RESET_IN` wird unmittelbar als `SET` bzw. `RESET` an `ASR_OUT` weitergereicht.

## Anwendungsszenarien

- **Zusammenführen zweier AE-Signalquellen** (z. B. aus [ASR_MERGE_2](../../../events/unidirectional/EVENT/ASR_MERGE_2.md) oder direkt von einem Sensor) zu einem einzigen ASR-Adapter für nachgeschaltete Set/Reset-Logik.
- **Adapterisierung** bestehender AE-basierter Netzwerke, um sie an ASR-Schnittstellen (z. B. `AX_SR`) anzubinden.

## Vergleich mit ähnlichen Bausteinen

- **[ASR_SR_TO_2AE](ASR_SR_TO_2AE.md)**: die Umkehrrichtung – zerlegt ein ASR-Signal wieder in zwei AE-Adapter.
- **[ASR_2EVENTS_TO_SR](ASR_2EVENTS_TO_SR.md)**: dieselbe Funktion mit klassischen Ereigniseingängen statt AE-Adapter-Sockets.
- **[ASRT_3AE_TO_SRT](ASRT_3AE_TO_SRT.md)**: die um `TOGGLE` erweiterte Variante für ASRT.

## Fazit

`ASR_2AE_TO_SR` ist eine einfache, reine Verdrahtungslösung zur Zusammenführung zweier AE-Adapter zu einem ASR-Adapter und eignet sich zur nahtlosen Integration von AE-basierten Ereignisquellen in ASR-Schnittstellen.

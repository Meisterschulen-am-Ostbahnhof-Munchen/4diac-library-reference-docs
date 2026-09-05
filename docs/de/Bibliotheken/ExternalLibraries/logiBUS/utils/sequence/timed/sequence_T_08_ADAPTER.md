# sequence_T_08_ADAPTER

![sequence_T_08_ADAPTER_network](./sequence_T_08_ADAPTER_network.svg)

* * * * * * * * * *

## Einleitung

`sequence_T_08_ADAPTER` ist ein Composite-Wrapper um [sequence_T_08](sequence_T_08.md): derselbe generische zeitgesteuerte 8-Schritt-Sequenzer, aber mit allen Ausgängen und Übergangszeiten auf Adapterverbindungen umgestellt. Anders als [sequence_T_08_AX](sequence_T_08_AX.md), das nur die acht Ausgänge auf `AX`-Adapter umstellt, bündelt `sequence_T_08_ADAPTER` zusätzlich die Zustandsnummer (`AS`) und führt jede der acht Übergangszeiten über einen eigenen `ATM`-Adapter-Socket ein.

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: sequence_T_08_ADAPTER

- **Typ**: Composite (FBNetwork)
- **Verwendete interne FBs**:
    - **Sequence**: `logiBUS::utils::sequence::timed::sequence_T_08` — der generische 8-Schritt-Zeitsequenzer, siehe [sequence_T_08](sequence_T_08.md).
    - **E_TimeOut**: `iec61499::events::E_TimeOut` — Standard-TimeOut-Adapter für die interne Zeitsteuerung; anders als bei `AnlagenSequenz_06_ADAPTER` genügt hier ein einzelner Standard-FB ohne zusätzlichen `TimeTicker`.
- **Funktionsweise**: `DO_S1`..`DO_S8` (Ausgänge je Zustand) und `STATE_NR` (aktuelle Zustandsnummer) laufen über `AX`- bzw. `AS`-Adapter; jede der acht Übergangszeiten `DT_S1_S2`..`DT_S8_START` wird über einen eigenen `ATM`-Adapter-Socket (`ATM_S1_S2`..`ATM_S8_START`) eingespeist statt als einzelner `TIME`-Dateneingang.

## Programmablauf und Verbindungen

1. `START_S1`/`RESET` werden unverändert an `Sequence.START_S1`/`Sequence.RESET` durchgereicht.
2. Jeder `ATM_Sx_Sy`-Adapter liefert seine Zeit (`.D1`) an `Sequence.DT_Sx_Sy` und löst gleichzeitig (`.E1`) das zugehörige `Sequence.SET_DT_Sx_Sy`-Setzereignis aus.
3. `Sequence.CNF` löst `STATE_NR.E1`, mit der aktuellen Zustandsnummer über `STATE_NR.D1`.
4. Jedes `Sequence.EO_Sx` löst `DO_Sx.E1`, mit dem Ausgangswert über `DO_Sx.D1`.
5. `Sequence.timeOut` (Adapter-Socket) ist intern mit `E_TimeOut.TimeOutSocket` verbunden — die Zeitsteuerung bleibt vollständig innerhalb des Composite gekapselt, im Gegensatz zu `sequence_T_08`/`sequence_T_08_AX`, die ihren `timeOut`-Adapter selbst nach außen führen.

## Anwendungsszenarien

- Anwendungen, die konsequent auf Adapterverbindungen setzen und weder einzelne `TIME`-Leitungen für die Übergangszeiten noch eine eigene `timeOut`-Adapterverbindung in der Anwendung ziehen wollen.
- Wiederverwendung des generischen 8-Schritt-Sequenzers in mehreren Anlagenteilen mit einheitlicher, adapterbasierter Verdrahtung.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- **[sequence_T_08](sequence_T_08.md)**: Die Basisvariante mit klassischen Event-/Daten-/Adapter-Anschlüssen (nur `timeOut` als Adapter).
- **[sequence_T_08_AX](sequence_T_08_AX.md)**: Stellt nur die acht Ausgänge auf `AX`-Adapter um, behält aber `TIME`-Dateneingänge für die Übergangszeiten und führt `timeOut` weiterhin selbst nach außen.
- **[AnlagenSequenz_06_ADAPTER](AnlagenSequenz_06_ADAPTER.md)**: Gleiches Adapter-Wrapper-Muster, angewendet auf die feste 6-Motoren-Ringtopologie statt auf den generischen 8-Schritt-Sequenzer.

## Fazit

`sequence_T_08_ADAPTER` macht den generischen 8-Schritt-Zeitsequenzer vollständig über Adapterverbindungen nutzbar — Ausgänge, Zustandsnummer, Übergangszeiten und Zeitsteuerung laufen ausschließlich über Adapter, keine einzelne Event-/Daten-Leitung verlässt den Baustein.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

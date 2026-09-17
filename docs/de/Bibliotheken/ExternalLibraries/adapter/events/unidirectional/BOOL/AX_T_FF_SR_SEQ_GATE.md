# AX_T_FF_SR_SEQ_GATE

Bistabiles Kettenglied-Flip-Flop mit Vorwärts-/Rückwärts-Verkettung (`ASR2`) und pegelbasierter Gate-Freigabe (`GATE:AX`).  
Ist `GATE.D1 = FALSE`, wird der Baustein zwangsweise auf `RESET` gesetzt und gesperrt (`GATE_CLOSED`). Alle Set- und Ketten-Set-Transitionen sind durch `[GATE.D1]` verriegelt.

## Interface

### Event Inputs

| Name | Comment |
| :--- | :------ |
| S | Lokaler Set-Eingang |
| R | Lokaler Reset-Eingang |
| CLK | Lokaler Toggle-Eingang |

### Adapters

| Name | Type | Direction | Comment |
| :--- | :--- | :-------- | :------ |
| Q | adapter::types::unidirectional::AX | Plug | Ausgangswert des Flip-Flops |
| CHAIN_IN | adapter::types::bidirectional::ASR2 | Socket | Ketten-Eingang |
| CHAIN_OUT | adapter::types::bidirectional::ASR2 | Plug | Ketten-Ausgang |
| GATE | adapter::types::unidirectional::AX | Socket | Freigabe-Signal (muss TRUE sein) |

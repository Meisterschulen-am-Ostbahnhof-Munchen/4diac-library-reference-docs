# AX_T_FF_SR_SEQ

Bistabiles Flip-Flop mit Kettenschaltung (`CHAIN_IN` / `CHAIN_OUT` über `ASR2`) für Vorwärts- und Rückwärts-Sequenzierung.

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

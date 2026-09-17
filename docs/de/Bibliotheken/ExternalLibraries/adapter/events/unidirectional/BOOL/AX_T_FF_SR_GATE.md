# AX_T_FF_SR_GATE

Bistabiles Flip-Flop mit Toggle-, Set- und Reset-Eingängen sowie einer pegelbasierten Gate-Freigabe (`GATE:AX`).  
Ist `GATE.D1 = FALSE`, wechselt der Baustein sofort in den Zustand `GATE_CLOSED` (Ausgang `Q` zwangsweise `FALSE`). Set-Operationen sind durch Guards (`[GATE.D1]`) direkt verriegelt.

## Interface

### Event Inputs

| Name | Comment |
| :--- | :------ |
| S | Set-Eingang |
| R | Reset-Eingang |
| CLK | Toggle-Eingang |

### Adapters

| Name | Type | Direction | Comment |
| :--- | :--- | :-------- | :------ |
| Q | adapter::types::unidirectional::AX | Plug | Ausgangswert des Flip-Flops |
| GATE | adapter::types::unidirectional::AX | Socket | Freigabe-Signal (muss TRUE sein) |

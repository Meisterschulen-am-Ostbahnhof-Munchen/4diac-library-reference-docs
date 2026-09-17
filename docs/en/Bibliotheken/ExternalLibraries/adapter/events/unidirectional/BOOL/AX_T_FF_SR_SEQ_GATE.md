# AX_T_FF_SR_SEQ_GATE

Bistable chain flip-flop with forward/backward sequencing (`ASR2`) and level-gated interlock (`GATE:AX`).  
If `GATE.D1 = FALSE`, the block forces `RESET` and locks (`GATE_CLOSED`). All set and chain-set transitions are explicitly guarded by `[GATE.D1]`.

## Interface

### Event Inputs

| Name | Comment |
| :--- | :------ |
| S | Local Set input |
| R | Local Reset input |
| CLK | Local Toggle input |

### Adapters

| Name | Type | Direction | Comment |
| :--- | :--- | :-------- | :------ |
| Q | adapter::types::unidirectional::AX | Plug | Flip-flop output value |
| CHAIN_IN | adapter::types::bidirectional::ASR2 | Socket | Chain input |
| CHAIN_OUT | adapter::types::bidirectional::ASR2 | Plug | Chain output |
| GATE | adapter::types::unidirectional::AX | Socket | Interlock gate signal (must be TRUE) |

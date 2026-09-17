# AX_T_FF_SR_SEQ

Bistable flip-flop with chain sequencing (`CHAIN_IN` / `CHAIN_OUT` via `ASR2`) supporting forward and backward propagation.

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

# AX_T_FF_SR_GATE

Bistable flip-flop with Toggle, Set, and Reset inputs and level-gated interlock (`GATE:AX`).  
If `GATE.D1 = FALSE`, the block immediately enters state `GATE_CLOSED` (output `Q` forced to `FALSE`). Set operations are explicitly guarded by `[GATE.D1]`.

## Interface

### Event Inputs

| Name | Comment |
| :--- | :------ |
| S | Set input |
| R | Reset input |
| CLK | Toggle input |

### Adapters

| Name | Type | Direction | Comment |
| :--- | :--- | :-------- | :------ |
| Q | adapter::types::unidirectional::AX | Plug | Flip-flop output value |
| GATE | adapter::types::unidirectional::AX | Socket | Interlock gate signal (must be TRUE) |

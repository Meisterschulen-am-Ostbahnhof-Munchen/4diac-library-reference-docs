# ASR2_DELAY

Independently delays Set and Reset events for a bidirectional `ASR2` chain segment in forward and backward directions.

## Interface

### Adapters

| Name | Type | Direction | Comment |
| :--- | :--- | :-------- | :------ |
| IN | adapter::types::bidirectional::ASR2 | Socket | Input Set/Reset |
| OUT | adapter::types::bidirectional::ASR2 | Plug | Output Set/Reset |
| STOP_SET_FWD | adapter::types::unidirectional::AE | Socket | Stop delay SET forward |
| PT_SET_FWD | adapter::types::unidirectional::ATM | Socket | Preset Time SET forward |
| STOP_SET_BWD | adapter::types::unidirectional::AE | Socket | Stop delay SET backward |
| PT_SET_BWD | adapter::types::unidirectional::ATM | Socket | Preset Time SET backward |
| STOP_RESET_FWD | adapter::types::unidirectional::AE | Socket | Stop delay RESET forward |
| PT_RESET_FWD | adapter::types::unidirectional::ATM | Socket | Preset Time RESET forward |
| STOP_RESET_BWD | adapter::types::unidirectional::AE | Socket | Stop delay RESET backward |
| PT_RESET_BWD | adapter::types::unidirectional::ATM | Socket | Preset Time RESET backward |

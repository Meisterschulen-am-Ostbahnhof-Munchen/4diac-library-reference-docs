# AE2_DELAY

Delays bidirectional event propagation (forward and backward) via `AE2` adapters.

## Interface

### Adapters

| Name | Type | Direction | Comment |
| :--- | :--- | :-------- | :------ |
| IN | adapter::types::bidirectional::AE2 | Socket | Incoming event |
| OUT | adapter::types::bidirectional::AE2 | Plug | Delayed outgoing event |
| STOP_FWD | adapter::types::unidirectional::AE | Socket | Stop delay forward |
| PT_FWD | adapter::types::unidirectional::ATM | Socket | Preset Time forward |
| STOP_BWD | adapter::types::unidirectional::AE | Socket | Stop delay backward |
| PT_BWD | adapter::types::unidirectional::ATM | Socket | Preset Time backward |

# ASR2_DELAY

Verzögert Set- und Reset-Ereignisse eines bidirektionalen `ASR2`-Kettenglieds unabhängig voneinander in Vorwärts- und Rückwärtsrichtung.

## Interface

### Adapters

| Name | Type | Direction | Comment |
| :--- | :--- | :-------- | :------ |
| IN | adapter::types::bidirectional::ASR2 | Socket | Eingangs-Set/Reset |
| OUT | adapter::types::bidirectional::ASR2 | Plug | Ausgangs-Set/Reset |
| STOP_SET_FWD | adapter::types::unidirectional::AE | Socket | Stop delay SET Vorwärts |
| PT_SET_FWD | adapter::types::unidirectional::ATM | Socket | Preset Time SET Vorwärts |
| STOP_SET_BWD | adapter::types::unidirectional::AE | Socket | Stop delay SET Rückwärts |
| PT_SET_BWD | adapter::types::unidirectional::ATM | Socket | Preset Time SET Rückwärts |
| STOP_RESET_FWD | adapter::types::unidirectional::AE | Socket | Stop delay RESET Vorwärts |
| PT_RESET_FWD | adapter::types::unidirectional::ATM | Socket | Preset Time RESET Vorwärts |
| STOP_RESET_BWD | adapter::types::unidirectional::AE | Socket | Stop delay RESET Rückwärts |
| PT_RESET_BWD | adapter::types::unidirectional::ATM | Socket | Preset Time RESET Rückwärts |

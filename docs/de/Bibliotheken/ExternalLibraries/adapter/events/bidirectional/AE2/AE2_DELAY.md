# AE2_DELAY

Verzögert die bidirektionale Ereignisweiterleitung (Vorwärts und Rückwärts) über `AE2`-Adapter.

## Interface

### Adapters

| Name | Type | Direction | Comment |
| :--- | :--- | :-------- | :------ |
| IN | adapter::types::bidirectional::AE2 | Socket | Ereignis vom vorherigen Kettenglied |
| OUT | adapter::types::bidirectional::AE2 | Plug | Verzögertes Ereignis an nächstes Kettenglied |
| STOP_FWD | adapter::types::unidirectional::AE | Socket | Stop delay Vorwärts |
| PT_FWD | adapter::types::unidirectional::ATM | Socket | Preset Time Vorwärts |
| STOP_BWD | adapter::types::unidirectional::AE | Socket | Stop delay Rückwärts |
| PT_BWD | adapter::types::unidirectional::ATM | Socket | Preset Time Rückwärts |

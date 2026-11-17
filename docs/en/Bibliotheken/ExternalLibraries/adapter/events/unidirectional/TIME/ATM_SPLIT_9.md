# ATM_SPLIT_9

![ATM_SPLIT_9](ATM_SPLIT_9.svg)

* * * * * * * * * *

!!! warning "Not released yet"
    The block is defined as a type in the library (as of 2026-11-13). The matching firmware side (C++ and ELF libraries) has not been released yet. Do not assume it is runnable yet.

## Introduction

`ATM_SPLIT_9` distributes **one** ATM adapter to 9 identical adapter outputs. The block is generic (`GEN_ATM_SPLIT`).

## Interface structure

### **Event inputs**

No direct events.

### **Event outputs**

No direct events.

### **Data inputs**

No direct data.

### **Data outputs**

No direct data.

### **Adapters**

| Role | Name | Type | Description |
| --- | --- | --- | --- |
| Socket | IN | adapter::types::unidirectional::ATM |  |
| Plug | OUT1 | adapter::types::unidirectional::ATM |  |
| Plug | OUT2 | adapter::types::unidirectional::ATM |  |
| Plug | OUT3 | adapter::types::unidirectional::ATM |  |
| Plug | OUT4 | adapter::types::unidirectional::ATM |  |
| Plug | OUT5 | adapter::types::unidirectional::ATM |  |
| Plug | OUT6 | adapter::types::unidirectional::ATM |  |
| Plug | OUT7 | adapter::types::unidirectional::ATM |  |
| Plug | OUT8 | adapter::types::unidirectional::ATM |  |
| Plug | OUT9 | adapter::types::unidirectional::ATM |  |

## How it works

The outputs are updated **only when the value changes**, the event is only sent on an actual change. For the variant without change detection there is `ATM_SPLIT_9_UNGATED`.

## Technical notes

- Generic block (`GenericClassName` = `GEN_ATM_SPLIT`), no network definition of its own in the type.
- The implementation is in the firmware (C++), not in the type.

## ⚖️ Comparison with similar blocks

- Other number of outputs: [ATM_SPLIT_5](ATM_SPLIT_5.md), [ATM_SPLIT_6](ATM_SPLIT_6.md), [ATM_SPLIT_7](ATM_SPLIT_7.md), [ATM_SPLIT_8](ATM_SPLIT_8.md).
- With or without change detection: [ATM_SPLIT_9_UNGATED](ATM_SPLIT_9_UNGATED.md).

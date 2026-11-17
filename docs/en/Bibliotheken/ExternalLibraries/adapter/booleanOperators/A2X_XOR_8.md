# A2X_XOR_8

![A2X_XOR_8](A2X_XOR_8.svg)

* * * * * * * * * *

!!! warning "Not released yet"
    The block is defined as a type in the library (as of 2026-11-13). The matching firmware side (C++ and ELF libraries) has not been released yet. Do not assume it is runnable yet.

## Introduction

`A2X_XOR_8` combines 8 [A2X](../types/unidirectional/BOOL/A2X.md) adapters with XOR, **UP and DOWN separately**. The block is generic (`GEN_A2X_XOR`), the number of inputs is part of the name.

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
| Socket | IN1 | adapter::types::unidirectional::A2X | XOR input 1 |
| Socket | IN2 | adapter::types::unidirectional::A2X | XOR input 2 |
| Socket | IN3 | adapter::types::unidirectional::A2X | XOR input 3 |
| Socket | IN4 | adapter::types::unidirectional::A2X | XOR input 4 |
| Socket | IN5 | adapter::types::unidirectional::A2X | XOR input 5 |
| Socket | IN6 | adapter::types::unidirectional::A2X | XOR input 6 |
| Socket | IN7 | adapter::types::unidirectional::A2X | XOR input 7 |
| Socket | IN8 | adapter::types::unidirectional::A2X | XOR input 8 |
| Plug | OUT | adapter::types::unidirectional::A2X | XOR result |

## How it works

The adapter output `OUT` is updated **only when the value changes**, the event is only sent on an actual change. For the variant without change detection there is `A2X_XOR_8_UNGATED`.

## Technical notes

- Generic block (`GenericClassName` = `GEN_A2X_XOR`), no network definition of its own in the type.
- UP and DOWN are combined separately.
- The implementation is in the firmware (C++), not in the type.

## ⚖️ Comparison with similar blocks

- Same block with another operation: [A2X_OR_8](A2X_OR_8.md).
- Same operation with another number of inputs: [A2X_XOR_2](A2X_XOR_2.md), [A2X_XOR_3](A2X_XOR_3.md), [A2X_XOR_4](A2X_XOR_4.md), [A2X_XOR_5](A2X_XOR_5.md), [A2X_XOR_6](A2X_XOR_6.md), [A2X_XOR_7](A2X_XOR_7.md).
- Single-channel predecessor: [AX_XOR_8](AX_XOR_8.md).

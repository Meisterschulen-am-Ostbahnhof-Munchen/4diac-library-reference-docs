# SPLIT_AQ_INTO_AX

![SPLIT_AQ_INTO_AX](SPLIT_AQ_INTO_AX.svg)

* * * * * * * * * *

!!! warning "Not released yet"
    The block is defined as a type in the library (as of 2026-11-13). The matching firmware side (C++ and ELF libraries) has not been released yet. Do not assume it is runnable yet.

## Introduction

`SPLIT_AQ_INTO_AX`: this Function splits the 2 AX (BOOL) adapters from a AQ (QUARTER).

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
| Socket | IN | adapter::types::unidirectional::AQ | QUARTER Input |
| Plug | BIT_00 | adapter::types::unidirectional::AX | Bool Bit 0 |
| Plug | BIT_01 | adapter::types::unidirectional::AX | Bool Bit 1 |

## How it works

The block is a service-interface block without a network of its own in the type. Its implementation is in the firmware.

## Technical notes

- Initial Version - Luecke in der Adapter-Familie geschlossen
- AQ is a `QUARTER` with 2 bits, `AX` is a bool adapter.

## ⚖️ Comparison with similar blocks

- Counterpart: [ASSEMBLE_AQ_FROM_AX](../assembling/ASSEMBLE_AQ_FROM_AX.md).

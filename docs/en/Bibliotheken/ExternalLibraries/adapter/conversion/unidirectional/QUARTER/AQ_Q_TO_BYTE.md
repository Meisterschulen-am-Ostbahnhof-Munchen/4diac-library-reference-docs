# AQ_Q_TO_BYTE

![AQ_Q_TO_BYTE](AQ_Q_TO_BYTE.svg)

![AQ_Q_TO_BYTE_network](./AQ_Q_TO_BYTE_network.svg)

* * * * * * * * * *

!!! warning "Not released yet"
    The block is defined as a type in the library (as of 2026-11-13). The matching firmware side (C++ and ELF libraries) has not been released yet. Do not assume it is runnable yet.

## Introduction

`AQ_Q_TO_BYTE` is a composite block: Composite FB for convert AQ to BYTE (QUARTER, 2 Bit).

## Interface structure

### **Event inputs**

No direct events.

### **Event outputs**

| Event | Type | With | Description |
| --- | --- | --- | --- |
| CNF | Event | IN | Confirmation from Adapter |

### **Data inputs**

No direct data.

### **Data outputs**

| Name | Type | Description |
| --- | --- | --- |
| IN | BYTE | Input data from Adapter |

### **Adapters**

| Role | Name | Type | Description |
| --- | --- | --- | --- |
| Socket | AQ_IN | adapter::types::unidirectional::AQ | Adapter Input |

## How it works

The connections are listed in the section “Connections inside the block”. There are no internal function blocks, the block only wires adapters and inputs/outputs.

## Connections inside the block

**Event connections:**

- `AQ_IN.E1` → `CNF`

**Data connections:**

- `AQ_IN.D1` → `IN`

## Technical notes

- Initial Version - Luecke geschlossen (analog zu AB_B_TO_BYTE)

## ⚖️ Comparison with similar blocks

Blocks of the same family are in the same folder or in the neighbouring folders of `conversion`.

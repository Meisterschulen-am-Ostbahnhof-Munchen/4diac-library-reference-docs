# AD2_DWORD_TO_D

![AD2_DWORD_TO_D](AD2_DWORD_TO_D.svg)

![AD2_DWORD_TO_D_network](./AD2_DWORD_TO_D_network.svg)

* * * * * * * * * *

!!! warning "Not released yet"
    The block is defined as a type in the library (as of 2026-11-13). The matching firmware side (C++ and ELF libraries) has not been released yet. Do not assume it is runnable yet.

## Introduction

`AD2_DWORD_TO_D` is a composite block: Composite FB for convert DWORD to AD2.

## Interface structure

### **Event inputs**

| Event | Type | With | Description |
| --- | --- | --- | --- |
| REQ | Event | OUT | Request to Adapter |

### **Event outputs**

| Event | Type | With | Description |
| --- | --- | --- | --- |
| CNF | Event | IN | Confirmation from Adapter |

### **Data inputs**

| Name | Type | Description |
| --- | --- | --- |
| OUT | DWORD | Output data to Adapter |

### **Data outputs**

| Name | Type | Description |
| --- | --- | --- |
| IN | DWORD | Input data from Adapter |

### **Adapters**

| Role | Name | Type | Description |
| --- | --- | --- | --- |
| Plug | AD2_OUT | adapter::types::bidirectional::AD2 | Adapter Output |

## How it works

The connections are listed in the section “Connections inside the block”. There are no internal function blocks, the block only wires adapters and inputs/outputs.

## Connections inside the block

**Event connections:**

- `REQ` → `AD2_OUT.EO1`
- `AD2_OUT.EI1` → `CNF`

**Data connections:**

- `OUT` → `AD2_OUT.DO1`
- `AD2_OUT.DI1` → `IN`

## Technical notes

- Initial Version - Luecke geschlossen (analog zu AB2_BYTE_TO_B)

## ⚖️ Comparison with similar blocks

Blocks of the same family are in the same folder or in the neighbouring folders of `conversion`.

# AIWS_WS_TO_WSTRING

![AIWS_WS_TO_WSTRING](AIWS_WS_TO_WSTRING.svg)

![AIWS_WS_TO_WSTRING_network](./AIWS_WS_TO_WSTRING_network.svg)

* * * * * * * * * *

!!! warning "Not released yet"
    The block is defined as a type in the library (as of 2026-11-13). The matching firmware side (C++ and ELF libraries) has not been released yet. Do not assume it is runnable yet.

## Introduction

`AIWS_WS_TO_WSTRING` is a composite block: Composite FB for convert AIWS to WSTRING.

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
| IN | WSTRING | Input data from Adapter |

### **Adapters**

| Role | Name | Type | Description |
| --- | --- | --- | --- |
| Socket | AIWS_IN | adapter::types::unidirectional::AIWS | Adapter Input |

## How it works

The connections are listed in the section “Connections inside the block”. There are no internal function blocks, the block only wires adapters and inputs/outputs.

## Connections inside the block

**Event connections:**

- `AIWS_IN.E1` → `CNF`

**Data connections:**

- `AIWS_IN.D1` → `IN`

## Technical notes

- Initial Version - Luecke geschlossen (analog zu AB_B_TO_BYTE)

## ⚖️ Comparison with similar blocks

Blocks of the same family are in the same folder or in the neighbouring folders of `conversion`.
